<properties
   pageTitle="API Entwurf Anleitungen | Microsoft Azure"
   description="Leitfäden nach zum Erstellen von a: entwickelt API."
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

# <a name="api-design-guidance"></a>API Entwurf Anleitungen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>(Übersicht)

Viele moderne webbasierten Lösungen Nutzen von Webdiensten, Webservern, eine für den remote-Client Applications Funktionalität befinden. Die Vorgänge, die von ein Webdienst verfügbar gemachten bilden eine Web-API. Eine durchdachte Web API sollte Ziele zu unterstützen:

- **Plattformunabhängigkeit**. Clientanwendungen sollten die API verwenden, die der Webdienst bereitstellt, ohne dass, wie die Daten oder die Vorgänge, die API verfügbar gemacht physisch implementiert werden können. Dies erfordert, dass die API gemeinsame Standards Richtlinien befolgt, mit denen einen Client-Anwendung und Web-Dienst sich einigen, welche Datenformaten verwenden, und die Struktur der Daten, die zwischen Clientanwendungen und dem Webdienst ausgetauscht werden.

- **Dienst Weiterentwicklung**. Der Webdienst sollte möglicherweise weiterentwickelt und hinzufügen (oder entfernen) Funktionalität unabhängig vom Clientanwendungen. Vorhandene Clientanwendungen sollte weiterhin unverändert als die Funktionen von der Änderung der Web-Dienst ausgeführt werden können. Alle Funktionen sollten auch sichtbar, sein, damit Clientanwendungen vollständig nutzen können.

Dieser Leitfaden dient die Probleme beschrieben, die Sie beim Entwerfen einer Webs-API berücksichtigen sollten.

## <a name="introduction-to-representational-state-transfer-rest"></a>Einführung in aussagekräftige State Transfer (REST)

In seinem Dissertation in 2000 vorgeschlagenen Roy Fielding einen alternativen Architektur Ansatz zum Strukturieren von die Vorgänge, Web Services bereitgestellt werden; REST. REST ist eine Architektur-Art für verteilte Systeme basierend auf Hypermedia erstellen. Ein wichtiger Vorteil der restlichen Modell ist, dass er auf offenen Standards basiert und bindet nicht die Implementierung von das Modell oder die Clientanwendungen, die sie den Zugriff auf eine bestimmte Implementierung. Beispielsweise ein REST-Webdienst mithilfe der Microsoft ASP.NET Web API implementiert werden konnte und Clientanwendungen können mithilfe einer beliebigen Sprache und Toolset, die HTTP-Anfragen generieren und Analysieren von HTTP-Antworten kann entwickelt werden.

> [AZURE.NOTE] REST ist tatsächlich unabhängig von einem beliebigen zugrunde liegenden Protokoll und nicht unbedingt an HTTP gebunden ist. Sie verwenden jedoch am häufigsten verwendete Implementierungen Systeme, die auf den restlichen basieren HTTP als Anwendungsprotokoll zum Senden und Empfangen von Besprechungsanfragen. Dieses Dokument befasst sich Systeme, die die Steuerung über HTTP REST Prinzipien zuordnen.

Das restlichen Modell verwendet ein nautische Schema zur Darstellung von Objekten und Diensten über ein Netzwerk (als _Ressourcen_bezeichnet). Viele Systeme, die in der Regel Implementieren der restlichen verwenden das HTTP-Protokoll Zugriff auf diese Ressourcen Anfragen zu übertragen. In diesen Systemen übermittelt eine Clientanwendung eine Anforderung in Form von einem URI, die eine Ressource bezeichnet, und HTTP-Methode (die am häufigsten verwendeten wird zu gelangen, Beitrag, setzen oder löschen), die den für die Ressource auszuführenden Vorgang angibt.  Hauptteil der HTTP-Anforderung enthält die Daten, die zum Ausführen des Vorgangs erforderlich. Wichtig zu verstehen ist, dass die restlichen ein Modells statusfreie Anforderung definiert. HTTP-Anfragen sollte unabhängig sein und können in beliebiger Reihenfolge auftreten, damit bei dem Versuch, vorübergehenden Zustand Informationsflusses zwischen Anfragen beibehalten nicht möglich ist.  Die einzige Stelle Informationen gespeichert in den Ressourcen selbst ist und jeder Anforderung sollten atomarer Vorgang. Ein REST Modell implementiert effektiv, einen begrenzten Zustand Computer, wo eine Anforderung eine Ressource von einem klar definierten dauerhaftes Zustand in einen anderen übergeht.

> [AZURE.NOTE] Einzelne Besprechungsanfragen in der restlichen Modell zustandsfrei sind ermöglicht ein Systems erstellt, die durch diese Prinzipien zum äußerst skalierbar folgen. Es gibt keine müssen alle Zugehörigkeit zwischen einer Clientanwendung, die eine Reihe von Besprechungsanfragen und den bestimmten Webservern behandeln diese Anfragen ausführenden beibehalten.

Ein weiterer entscheidender Punkt bei der Durchführung eines effektiven restlichen Modells besteht darin, die Beziehungen zwischen den verschiedenen Ressourcen zu verstehen, der das Modell Access bietet. Diese Ressourcen werden als Websitesammlungen und Beziehungen in der Regel angezeigt. Nehmen Sie beispielsweise an, dass eine schnelle Analyse von einem e-Commerce-System zeigt, dass es gibt zwei Websitesammlungen in der sich Client Applications sind wahrscheinlich interessiert: Bestellungen und Kunden. Jede Bestellung und Kunden sollte einen eigene eindeutigen Schlüssel zur Identifizierung verfügen. Der URI Zugriff auf die Auflistung der Bestellungen könnte ungefähr so einfach wie das _/orders_, und entsprechend der URI für alle Kunden abrufen _"/ Customers"_werden konnte. Ausgabe von HTTP GET-Anforderung an das _/orders_ URI sollte eine Liste aller Aufträge in der Auflistung als eine HTTP-Antwort codierte darstellt zurück:

```HTTP
GET http://adventure-works.com/orders HTTP/1.1
...
```

Im unten gezeigten Antwort codiert die Aufträge als eine JSON-Datenstruktur Liste:

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
[{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1},{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2},{"orderId":3,"orderValue":16.60,"productId":2,"quantity":4},{"orderId":4,"orderValue":25.90,"productId":3,"quantity":1},{"orderId":5,"orderValue":99.90,"productId":1,"quantity":1}]
```
Zum Abrufen von einzelnen Ordnung erfordert, dass den Bezeichner für den Auftrag aus der _Orders_ -Ressource, z. B. _/orders/2_:

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
```

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2}
```

> [AZURE.NOTE] Zur Vereinfachung in diesen Beispielen wird die Informationen in Antworten als JSON-Textdaten zurückgegeben wird angezeigt. Es ist jedoch kein Grund, warum eine andere Art von Daten über HTTP, wie z. B. binäre oder verschlüsselten Informationen unterstützt Ressourcen nicht enthalten soll. der Inhaltstyp in der HTTP-Antwort sollte den Typ angeben. Darüber hinaus ein REST-Modells zurückzugebenden dieselben Daten in verschiedenen Formaten, wie XML oder JSON möglicherweise. In diesem Fall sollte der Webdienst Inhalt verhandeln mit der Client, die Anforderung ausführen können. Die Anforderung kann einer Kopfzeile _akzeptieren_ enthalten, gibt das bevorzugte Format an, die der Client möchte erhalten und der Webdienst sollten versuchen, dieses Format abwickeln, falls möglich.

Beachten Sie, dass die Antwort einer Anforderung REST Verwenden des standardmäßigen HTTP-Statuscodes. Sollte eine Anforderung, die gültige Daten gibt kann beispielsweise den HTTP-Antwortcode 200 (OK), während eine Anforderung, die fehlschlägt, suchen oder Löschen einer angegebenen Ressource eine Antwort zurückgegeben wird sollte, die den HTTP-Statuscode 404 (nicht gefunden) einschließt.

## <a name="design-and-structure-of-a-restful-web-api"></a>Entwerfen und Struktur der ein Rest Web-API

Der Schlüssel zum Entwerfen einer erfolgreichen Web-API sind Übersichtlichkeit und Konsistenz. Eine Web-API, die folgenden zwei Faktoren weist vereinfacht Clientanwendungen erstellen, die die API nutzen müssen.

Eine Rest Web API konzentriert sich eine Reihe von verbundenen Ressourcen verfügbar zu machen und Bereitstellen von Core-Operationen, mit die eine Anwendung können Sie diese Ressourcen bearbeiten und einfach dazwischen navigieren können. Daher sollten die URIs, die ein Rest Webdienst API bilden ausgerichtet in Richtung der Daten, die verfügbar gemacht werden, und verwenden die Funktionen von HTTP auf diese Daten angewendet werden. Dieser Ansatz erfordert eine andere Stelle aus, die in der Regel beschäftigt, wenn eine Reihe von Klassen in einer objektorientierten API entwerfen, wodurch mehr durch das Verhalten von Objekten und Klassen davon werden. Darüber hinaus sollte eine Rest Web API statusfrei sein und keine Vorgänge, die in einer bestimmten Reihenfolge aufgerufen abhängig. In den folgenden Abschnitten werden die Punkte, die Sie beim Entwerfen einer Rest Webs API berücksichtigen sollten.

### <a name="organizing-the-web-api-around-resources"></a>Organisieren von im Web API um Ressourcen

> [AZURE.TIP] Sollte die URIs verfügbar gemacht werden, von einem Webdienst REST Substantive Grundlage (die Daten ermöglicht den Zugriff auf die im Web-API) und nicht für Verben (Anwendung mit den Daten Möglichkeiten).

Fokussierung auf die geschäftlichen Faktoren, die im Web-API macht. Die primäre Elemente sind in einem Web-API soll die zuvor beschriebene e-Commerce-System unterstützen, z. B. Kunden und Bestellungen. Prozesse wie der Vorgang, eine Bestellung aufgeben können können, indem Sie ein HTTP POST-Vorgang, der übernimmt die Reihenfolge Informationen und fügt es in die Liste der Aufträge für den Kunden, erzielt werden. Intern, können diesen Beitrag Aufgaben wie überprüfen vorhandenen Ebenen und den Kunden Abrechnung Vorgang. HTTP-Antwort haben, anzugeben, ob die Reihenfolge erfolgreich oder nicht platziert wurde. Beachten Sie auch, dass eine Ressource nicht auf ein einzelnes physische Datenelement basieren. Beispielsweise kann eine Ressource Reihenfolge intern mithilfe von Informationen aus der Anzahl von Zeilen, die auf mehrere Tabellen in einer relationalen Datenbank verteilt, jedoch auf dem Client als einzelne Einheit angezeigt zusammengefasster implementiert werden.

> [AZURE.TIP] Vermeiden Sie das Entwerfen einer REST-Benutzeroberfläche, die spiegelt oder hängt von der internen Struktur der Daten, die verfügbar gemacht. REST ist zu implementieren mehr als einfache (erstellen, abrufen, aktualisieren, löschen) Vorgänge über separate Tabellen in einer relationalen Datenbank. Der Zweck der restlichen ist ordnen Sie diese physische Details Business Einheiten und die Vorgänge, die eine Anwendung auf diese Elemente aus, um die physische Implementierung von diesen Einheiten ausführen kann, aber kein Client verfügbar gemacht werden.

Einzelne Unternehmen weiter selten vorhanden sein, isoliert (auch einige einzelne Objekte möglicherweise vorhanden ist), aber stattdessen in der Regel in Websitesammlungen zusammengefasst werden. Im weiteren Verlauf sind Ausdrücke, jede Entität und jede Websitesammlung Ressourcen. Rest Web API jede Websitesammlung verfügt über eine eigene URI innerhalb des Webdiensts und Durchführung einer HTTP GET-Anforderung über einen URI für eine Websitesammlung Ruft eine Liste der Elemente in dieser Sammlung. Jedes einzelnes Element verfügt auch über eine eigene URI, und eine Anwendung kann ein anderes diesen URI verwenden, um die Details des betreffenden Elements abzurufen HTTP GET-Anforderung senden. Sie sollten die URIs für Websitesammlungen und Elemente hierarchisch anordnen. E-Commerce-System der URI _"/ Customers"_ eine Auflistung der vom Kunden und _/customers/5_ Ruft die Details für die einzelnen Kunden mit der 5-ID aus dieser Auflistung ab. Dieser Ansatz hilft, die im Web API intuitive beibehalten.

> [AZURE.TIP] Übernehmen Sie eine einheitliche Benennungskonvention in URIs; im Allgemeinen sollten sie Pluralformen Substantive für URIs dieser Bezug Websitesammlungen verwenden.

Sie benötigen ferner zu berücksichtigen ist die Beziehungen zwischen verschiedenen Arten von Ressourcen und wie Sie diese Zuordnungen möglicherweise zugänglich gemacht wird. Kunden gelten möglicherweise 0 (null) oder mehrere Bestellungen. Durch einen URI wie _/customers/5/orders_ wäre natürliche Möglichkeit zum Darstellen dieser Beziehung um alle Aufträge für Kunden 5 zu finden. Sie sollten die Zuordnung in Ordnung wieder in einen bestimmten Kunden durch einen URI wie _/orders/99/customer_ zum Suchen des Kunden für Reihenfolge 99 darstellt, aber dieses Modell zu weit verlängern kann schwerfällig implementiert werden. Eine bessere Lösung ist Navigation Links zu anbieten Groove Ressourcen, wie etwa die Kunden, in den Textkörper der HTTP-Antwort zurückgegeben wird, wenn die Reihenfolge abgefragt wird. Dieses Verfahren ist im Abschnitt mit der HATEOAS Ansatz zum Aktivieren der Navigation zu zugehörige Ressourcen weiter unten in diesem Handbuch ausführlicher beschrieben.

Komplexeren Systemen können viele weitere Typen von Entität vorhanden sein, und es verlockend, bieten URIs, mit denen eine Clientanwendung zum Navigieren in mehrere Ebenen von Beziehungen, z. B. _/customers/1/orders/99/products_ , um eine Liste der Produkte in Reihenfolge nach Kunde 1 platziert 99 erhalten werden kann. Dieser Grad der Komplexität kann jedoch schwer zu verwalten und ist feste, wenn die Beziehungen zwischen Ressourcen in der Zukunft ändern. Sie sollten vielmehr Zielwertsuche URIs relativ einfachen beibehalten. Tragen Sie beachten Sie, dass sobald eine Anwendung einen Verweis auf eine Ressource verfügt, verwenden diesen Verweis zum Suchen von Elementen, die im Zusammenhang mit dieser Ressource werden sollte. Die vorherige Abfrage kann mit den URI- _/customers/1/orders_ finden alle Bestellungen für Kunden 1 und dann Abfragen, die URI- _/orders/99/products_ zum Ermitteln der Produkte in dieser Reihenfolge (vorausgesetzt, dass die Reihenfolge 99 nach Kunde 1 platziert wurde) ersetzt werden.

> [AZURE.TIP] Vermeiden Sie es mit Anforderung der Ressource URIs komplexer als _Auflistung/Element/Auflistung_.

Ein weiterer wichtiger Aspekt ist, dass alle Webanfragen eine Last auf dem Webserver bedeuten, und desto größer die Anzahl der desto größer die laden anfordert. Versuchen Sie zum Definieren von Ressourcen zur Vermeidung von "detailliert, sondern grob" Web APIs, die eine große Anzahl von kleinen Ressourcen verfügbar machen. Eine-API erfordern möglicherweise eine Clientanwendung und übermitteln Sie mehrere Anforderungen, um alle Daten zu finden, die erforderlich ist. Sie können Daten Denormalisieren und kombinieren Weitere Informationen zu vergrößern Ressourcen, die abgerufen werden können, indem Sie eine einzelne Anforderung hilfreich sein. Jedoch müssen Sie abwägen dieser Ansatz gegen den Verwaltungsaufwand Abrufen von Daten, die nicht häufig vom Client möglicherweise benötigt. Abrufen von großen Objekten können Sie vergrößern die Wartezeit einer Anforderung und anfallen zusätzliche Bandbreitenkosten für kleine nutzen, wenn die zusätzlichen Daten nicht häufig verwendet werden.

Vermeiden Sie Einführung in Abhängigkeiten zwischen im Web API, um die Struktur, Typ oder Ort der zugrunde liegenden Datenquellen. Angenommen, wenn die Daten in einer relationalen Datenbank gespeichert ist, muss die Web-API nicht jede Tabelle als eine Sammlung von Ressourcen verfügbar zu machen. Stellen Sie sich im Web API als ein Modell für die Datenbank, und bei Bedarf vorstellen eines Layers Zuordnung zwischen der Datenbank und im Internet-API. Auf diese Weise ändert sich der Entwurf oder Implementierung der Datenbank (z. B. Sie verschieben aus einer relationalen Datenbank, die eine Auflistung von standardisierten Tabellen zu einem denormalisierte NoSQL Speichersystem, wie etwa einer Datenbank Dokument enthält) Clientanwendungen aus diesen Änderungen isoliert sind.
> [AZURE.TIP] Die Quelle der Daten, die eine Web-API unterstützt hat keinen Datenspeicher sein; dies möglicherweise ein anderer Dienst oder Line-of-Business-Anwendung oder sogar eine ältere Anwendung ausführen lokalen innerhalb einer Organisation.

Schließlich dies möglicherweise nicht möglich, ordnen Sie jedem Vorgang, von einer Web-API auf eine bestimmte Ressource implementiert verwenden. Sie können eine solche Szenarien _nicht ressourcenbezogenen_ über HTTP GET-Anfragen behandeln, die einen Teil der Funktionen aufrufen und die Ergebnisse als eine Antwortnachricht HTTP zurückzugeben. Eine Web-API, die einfachen Rechner-Stil Operationen wie implementiert Addition und Subtraktion könnte URIs, die folgenden Vorgänge als Pseudo Ressourcen verfügbar machen und nutzen die Abfragezeichenfolge zum Angeben der erforderlichen Parameter bereitstellen. Beispielsweise eine GET-Anforderung an den URI _/add?operand1 = 99 & operand2 = 1_ konnte eine Antwortnachricht an die Stelle, mit dem Wert 100 zurückzukehren, und GET-Anforderung an den URI _/subtract?operand1 = 50 & operand2 = 20_ konnte eine Antwortnachricht an die Stelle, mit dem Wert 30 zurück. Jedoch nur verwenden Sie diese Formen von URIs nur in Ausnahmefällen.

### <a name="defining-operations-in-terms-of-http-methods"></a>Definieren von Vorgängen im Hinblick auf HTTP-Methoden

Das HTTP-Protokoll definiert eine Reihe von Methoden, die eine Anforderung semantische Bedeutung zuweisen. Am häufigsten Rest Web APIs verwendeten wird die allgemeinen HTTP-Methoden sind:

- **Abrufen**, um eine Kopie der Ressource am angegebenen URI abzurufen. Hauptteil der Antwortnachricht enthält die Details der angeforderten Ressource an.

- **Bereitstellen**, um eine neue Ressource am angegebenen URI zu erstellen. Hauptteil der Anforderungsnachricht enthält die Details der neuen Ressource an. Beachten Sie, dass Beitrag auch Vorgänge ausgelöst, die tatsächlich Ressourcen erstellen nicht verwendet werden kann.

- **Setzen**, ersetzen oder aktualisieren die Ressource am angegebenen URI. Hauptteil der Anforderungsnachricht gibt an, der Ressource korrigiert werden muss und die Werte angewendet werden soll.

- **Löschen**die Ressource am angegebenen URI zu entfernen.

> [AZURE.NOTE] Definiert das HTTP-Protokoll auch andere weniger häufig verwendeten Methoden, wie PATCH, der zum selektiven Updates an eine Ressource anfordern, LEITEN, die verwendet wird, um eine Beschreibung der eine Ressource, Optionen eine Clientinformationen zum Abrufen von Informationen über die Kommunikationsoptionen unterstützt, die vom Server, wodurch anfordern verwendet wird, und verfolgen, wodurch einen Client das Anfordern von Informationen, die sie für Test- und Diagnose verwenden können.

Das Verhalten einer bestimmten Anforderung sollte abhängig, ob die Ressource für die gilt eine Auflistung oder ein einzelnes Element ist. In der folgenden Tabelle werden die allgemeinen Konventionen von am häufigsten Rest unter Verwendung des Beispiels e-Commerce-Implementierungen angenommenen zusammengefasst. Beachten Sie, dass nicht für alle diese Anfragen implementiert werden könnte. Das hängt von des jeweiligen Szenarios ab.

| **Ressource** | **Bereitstellen** | **Erhalten** | **SETZEN** | **LÖSCHEN** |
|--------------|----------|---------|---------|------------|
| "/ Customers" | Erstellen Sie einen neuen Kunden | Alle Kunden abrufen | Gleichzeitig aktualisieren des Kunden (_Falls implementiert_) | Entfernen Sie alle Kunden |
| / Kunden/1 | Fehler | Rufen Sie die Details für Kunden 1 | Aktualisieren Sie die Details des Kunden 1 Falls vorhanden, andernfalls Eingabe einen Fehler | Entfernen von Kunden 1 |
| /Customers/1/Orders | Erstellen eines neuen Auftrags für Kunden 1 | Abrufen von allen Bestellungen für Kunden 1 | Gleichzeitig aktualisieren der Bestellungen für Kunden 1 (_Falls implementiert_) | Entfernen Sie alle Aufträge für Kunden 1 (_Falls implementiert_) |

Der Zweck des abrufen und Löschen von Besprechungsanfragen sind relativ einfach, doch es gibt Bereich für Verwirrung betreffend den Zweck und Effekte der Beitrag und sich Anfragen.

Eine POST-Anforderung sollte eine neue Ressource mit Daten in den Textkörper der Anfrage bereitgestellten erstellen. Im restlichen Modell wenden Sie häufig POST-Anfragen auf Ressourcen, die Websitesammlungen sind; die neue Ressource wird der Auflistung hinzugefügt.

> [AZURE.NOTE] Sie können auch definieren POST-Anfragen, die einige Funktionen auslösen (und, die nicht unbedingt Daten zurück), und diese Typen von Anforderung Sammlungen angewendet werden können. Beispielsweise können Sie eine POST-Anforderung eine Arbeitszeittabelle an einen Payroll Verarbeitungsdienst übergeben und die berechneten Steuern wieder als Antwort.

Anforderung einer sich soll eine vorhandene Ressource zu ändern. Wenn die angegebene Ressource nicht vorhanden ist, kann die Anfrage sich einen Fehler zurück (in einigen Fällen möglicherweise tatsächlich Erstellung die Ressource). VERSETZT Anfragen werden am häufigsten auf Ressourcen angewendet, die einzelne Elemente (beispielsweise eine bestimmte Kunden oder Reihenfolge), sind, obwohl sie Websitesammlungen, angewendet werden können, obwohl dies weniger häufig implementiert ist. Beachten Sie, die sich Idempotent sind anfordert, während Sie POST-Anfragen nicht sind. Wenn eine Anwendung die gleiche sendet setzen Anforderung mehrmals die Ergebnisse sollten immer gleich sein (dieselbe Ressource wird mit den gleichen Werten geändert), aber wenn eine Anwendung die gleiche Beitrag Anforderung wiederholt werden das Ergebnis der Erstellung mehrerer Ressourcen.

> [AZURE.NOTE] Genau genommen ersetzt eine Anforderung HTTP setzen eine vorhandene Ressource mit den Ressourcen im Textkörper der Anfrage angegeben. Wenn Sie die Absicht besteht darin, eine Auswahl von Eigenschaften in einer Ressource zu ändern, sondern andere Eigenschaften unverändert lassen, sollte dies implementiert per Anforderung HTTP-PATCH. Allerdings viele Rest Implementierungen mit dieser Regel entspannen und sich in beiden Fällen verwenden.

### <a name="processing-http-requests"></a>Verarbeitung von HTTP-Anfragen
Die Daten, die im Lieferumfang von einer Clientanwendung in viele HTTP-Anfragen und die entsprechende Antwortnachrichten aus dem Webserver konnte in einer Vielzahl von Formaten (oder Medientypen) angezeigt werden. Beispielsweise könnten die Daten, die angibt, die Details für einen Kunden oder die Reihenfolge als XML, JSON oder ein anderes Format codierten und komprimierte bereitgestellt werden. Eine Rest Web API sollten unterschiedliche Medientypen unterstützen, wie von der Clientanwendung angefordert, die eine Anforderung übermittelt.

Wenn eine Clientanwendung eine Anforderung, die Daten in den Textkörper einer Nachricht zurückgibt sendet, können sie die Medientypen, die sie behandelt werden kann, in der Kopfzeile annehmen der Anfrage angeben. Der folgende Code zeigt eine HTTP GET-Anforderung, die die Details des Kunden 1 ruft und das Ergebnis als JSON zurückgegeben werden soll (der Client sollten immer noch die Medientypen der Daten in der Antwort auf das Format der zurückgegebenen Daten überprüfen untersuchen) anfordert:

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
Accept: application/json
...
```

Wenn der Webserver diese Medientypen unterstützt, können sie mit einer Antwort Antworten, die Inhaltstyp-Header enthält, die angibt, das Format der Daten in den Textkörper der Nachricht:

> [AZURE.NOTE] Für optimale Interoperabilität erkannt die Medien, die Spaltenüberschriften akzeptieren und Inhaltstyp referenzierte Typen sollten MIME-Typen, anstatt einige benutzerdefinierten Medien einzugeben.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
```

Wenn der Webserver die angeforderten Medientypen nicht unterstützt, können sie die Daten in einem anderen Format senden. IN allen Fällen müssen sie die Medientypen (z. B. _Anwendung/Json_) in der Kopfzeile Inhaltstyp angeben. Es ist den Zuständigkeitsbereich der Clientanwendung zu analysieren, die Antwortnachricht ordnungsgemäß bei der Interpretation der Ergebnisse in den Nachrichtentext.

Beachten Sie, dass in diesem Beispiel Webserver erfolgreich Ruft die angeforderten Daten ab und Erfolg gibt durch wieder einen Statuscode von 200 in der Kopfzeile Antwort übergeben. Wenn keine übereinstimmenden Daten gefunden werden, sollten sie stattdessen einen Statuscode der 404 (nicht gefunden) zurückgeben und der Hauptteil der Antwortnachricht kann zusätzliche Informationen enthalten. Das Format dieser Informationen wird durch die Kopfzeile Inhaltstyp angegeben, wie im folgenden Beispiel gezeigt:

```HTTP
GET http://adventure-works.com/orders/222 HTTP/1.1
...
Accept: application/json
...
```

Reihenfolge 222 ist nicht vorhanden, sodass die Antwortnachricht wie folgt aussieht:

```HTTP
HTTP/1.1 404 Not Found
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"message":"No such order"}
```

Wenn eine Anwendung eine Anforderung HTTP setzen, um eine Ressource aktualisieren sendet, gibt den URI der Ressource und stellt die Daten in den Textkörper der Anforderungsnachricht geändert werden. Sie sollten das Format dieser Daten auch mithilfe des Inhaltstyp-Headers angeben. Ein gebräuchliches Format für textbasierten Informationen verwendet wird _Anwendung/X-www-form-urlencoded_, die eine Reihe von Name/Wert-Paare durch Trennzeichen getrennte besteht aus den & Zeichen. Im nächste Beispiel zeigt eine setzen HTTP-Anforderung, die die Informationen in der Reihenfolge 1 ändert:

```HTTP
PUT http://adventure-works.com/orders/1 HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
ProductID=3&Quantity=5&OrderValue=250
```

Wenn die Änderung erfolgreich ist, sollten sie idealerweise Antworten mit einem HTTP-204 Status-Code, der angibt, dass der Vorgang erfolgreich behandelt wurde, aber die Antwort Textkörper keine weiteren Informationen enthält. Location-Header in der Antwort enthält den URI der aktualisierten Ressource an:

```HTTP
HTTP/1.1 204 No Content
...
Location: http://adventure-works.com/orders/1
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

> [AZURE.TIP] Wenn die Daten in einer HTTP setzen Anforderungsnachricht Datum und die Uhrzeit enthält, stellen Sie sicher, dass Ihr Webdienst Datumsangaben akzeptiert und Uhrzeiten formatiert folgen den Standard ISO 8601.

Wenn die Ressource aktualisiert werden, nicht vorhanden ist, kann der Webserver mit einer Antwort nicht gefunden Antworten, wie zuvor beschrieben. Wenn der Server tatsächlich auf das Objekt selbst erstellt konnte er Alternativ Zurückgeben der Status codes HTTP 200 (OK) oder HTTP 201 (erstellt) und den Hauptteil einer Antwort konnte die Daten für die neue Ressource enthalten. Wenn die Kopfzeile Inhaltstyp der Anfrage ein Datenformat, die der Webserver behandeln kann angibt, sollte er HTTP-Statuscode 415 (nicht unterstützte Medientypen) zurück.

> [AZURE.TIP] Erwägen Sie das Implementieren Massen HTTP sich Vorgänge, die Updates für mehrere Ressourcen in einer Websitesammlung Stapel können. Die Anfrage sich sollte Geben Sie den URI der Sammlung und den Hauptteil der Anforderung sollte die Details der Ressourcen, die geändert werden. Dieser Ansatz kann dazu beitragen Funknetzen verringern und die Leistung zu verbessern.

Das Format von einer HTTP POST-Anfragen, die neue Ressourcen erstellen sind ähnlich wie die der sich Anfragen; Nachrichtentext enthält die Details der neuen Ressource hinzugefügt werden soll. Jedoch gibt den URI normalerweise der Websitesammlung, der die Ressource hinzugefügt werden soll. Im folgende Beispiel erstellt eine neue Bestellung, und fügt es der Orders-Auflistung hinzu:

```HTTP
POST http://adventure-works.com/orders HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
productID=5&quantity=15&orderValue=400
```

Wenn die Anforderung erfolgreich ist, sollte der Webserver mit einer Nachricht Code mit HTTP-Statuscode 201 (erstellt) reagieren. Location-Header sollte den URI der neu erstellten Ressource enthalten sollte, und der Hauptteil der Antwort eine Kopie der neuen Ressource; der Inhaltstyp-Header gibt das Format dieser Daten an:

```HTTP
HTTP/1.1 201 Created
...
Content-Type: application/json; charset=utf-8
Location: http://adventure-works.com/orders/99
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":99,"productID":5,"quantity":15,"orderValue":400}
```

> [AZURE.TIP] Wenn die Daten durch eine Anforderung sich oder Beitrag bereitgestellten ungültig ist, sollte der Webserver mit einer Nachricht mit HTTP-Statuscode 400 (Ungültige Anforderung) reagieren. Hauptteil dieser Meldung kann zusätzliche Informationen zu dem Problem mit der Anfrage und den Formaten erwartet enthalten, oder sie können einen Link zu einer URL, die weitere Details enthält enthalten.

Zum Entfernen einer Ressource bietet eine Anforderung HTTP löschen einfach den URI der Ressource gelöscht werden soll. Im folgenden Beispiel wird versucht, Reihenfolge 99 zu entfernen:

```HTTP
DELETE http://adventure-works.com/orders/99 HTTP/1.1
...
```

Wenn der Löschvorgang erfolgreich ist, sollte der Webserver reagiert mit HTTP-Statuscode 204, das angibt, dass der Vorgang erfolgreich behandelt wurde, aber, dass der Textkörper der Antwort enthält keine weiteren Informationen (Dies ist dieselbe Antwort zurückgegebene einen erfolgreichen sich Vorgang, aber ohne eine Kopfzeile Speicherort wie die Ressource ist nicht mehr vorhanden.) Es kann auch für eine Anforderung löschen HTTP-Statuscode 200 (OK) oder 202 (akzeptiert) zurück, wenn der Löschvorgang asynchrone durchgeführt werden.

```HTTP
HTTP/1.1 204 No Content
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

Wenn die Ressource nicht gefunden wird, sollten der Webserver stattdessen eine 404 (nicht gefunden) Nachricht zurückgeben.

> [AZURE.TIP] Wenn alle Ressourcen in einer Websitesammlung werden gelöscht müssen, aktivieren Sie eine Anforderung HTTP löschen, um nach dem URI für Erzwingen der Anwendung zu jeder Ressource wiederum aus der Auflistung entfernen, anstatt die Sammlung angegeben werden muss.

### <a name="filtering-and-paginating-data"></a>Filterung und Paginieren Daten

Sie sollten die URIs einfache und intuitive beibehalten bemüht. Verfügbarmachen einer Sammlung von Ressourcen über einen einzigen URI in diesem Zusammenhang unterstützt, aber es kann dazu führen, dass Applikationen abrufen große Datenmengen aus, wenn nur eine Teilmenge der Informationen erforderlich ist. Eine große Anzahl von Datenverkehr generieren wirkt sich auf die nicht nur die Leistung und Skalierbarkeit des Web-Servers aber auch beeinträchtigen der Reaktionszeiten Clientanwendungen angeforderten Daten.

Beispielsweise enthalten Bestellungen für die Reihenfolge bezahlten Preis, müssen eine Clientanwendung, die alle Aufträge abrufen, die Kosten über einen bestimmten Wert aufweisen muss alle Aufträge aus _/orders_ URI abrufen und dann diese Aufträge lokal zu filtern. Dieses Verfahren wird klar hochgradig nicht effizient; Es verschwendet Netzwerk Bandbreite und Verarbeitung Power auf dem Server mit dem Internet-API.

Eine Lösung möglicherweise ein URI-Schema, wie z. B. _/orders/ordervalue_greater_than_n_ , wobei _n_ der Reihenfolge Kurs ist, sondern auch für alle bereitstellen, aber eine eingeschränkte Anzahl von Preisen ein solcher Ansatz ist unbrauchbar. Darüber hinaus, wenn Sie Abfrage Bestellungen basierend auf anderen Kriterien müssen, können Sie einhandeln mit bieten eine lange Liste mit URIs mit möglicherweise nicht aussagekräftigen Namen konfrontiert wird.

Stellen Sie eine bessere Strategie zum Filtern von Daten die Filterkriterien in der Abfragezeichenfolge bereit, die im Web-API, wie weitergegeben wird _/orders?ordervaluethreshold n =_. In diesem Beispiel ist der entsprechende Vorgang im Web API verantwortlich für die Analyse und Behandlung der `ordervaluethreshold` Parameter in der Abfragezeichenfolge und die gefilterten Ergebnisse in der HTTP-Antwort zurückgegeben.

Einige einfachen HTTP GET-Anfragen über die Sammlung Ressourcen konnte potenziell eine große Anzahl von Elementen zurück. Wenn Sie die Möglichkeit, diese auftritt Bekämpfung sollten im Web-API, um den Umfang der Daten, die eine einzelne Anforderung zurückgegebene beschränken entwerfen. Dies können Sie erreichen durch die Unterstützung der Abfragezeichenfolgen, mit denen den Benutzer aus, um anzugeben, die maximale Anzahl von Elementen, die abgerufen werden können (sich selbst unterliegen einer Obergrenze hinsichtlich der DOS Angriffen verhindern handeln), und ein Anfangs-Offset in die Sammlung. Beispielsweise der Abfragezeichenfolge im URI _/orders?limit = 25 & Offset = 50_ abrufen 25 Bestellungen, beginnend mit der 50 % Reihenfolge, in der Auflistung Bestellungen gefunden werden soll. Wie beim Filtern von Daten, der Vorgang, der GET-Anforderung im Web API implementiert für die Analyse und Behandlung verantwortlich ist die `limit` und `offset` Parameter in der Abfragezeichenfolge. Um Clientanwendungen unterstützen, erhalten Sie Besprechungsanfragen, die zurückgeben, dass die Seitennummerierung Daten auch Metadaten enthalten soll, die die Gesamtzahl der in der Auflistung verfügbaren Ressourcen angeben. Sie sollten andere intelligente Seitennavigation Strategien; Weitere Informationen finden Sie unter [-API Design Notes: intelligentes Paging](http://bizcoder.com/api-design-notes-smart-paging)

Sie können eine ähnliche Strategie zum Sortieren von Daten wie dem Abruf ist folgen. könnten Sie Sortierparameter, die einen Feldnamen als Wert, z. B. akzeptiert bereitstellen _/orders?sort ProductID =_. Beachten Sie jedoch, dass dieser Ansatz zeitliche Zwischenspeichern (Abfrage, dass die Zeichenfolgenparameter Teil der Resource Identifier, indem Sie viele Cache Implementierungen als Schlüssel auf zwischengespeicherte Daten bilden) werden kann.

Die Felder dieser Ansatz schränken (Projekt) erweitern zurückgegeben, wenn ein Element zu einer Ressource eine große Datenmenge enthält. Sie können beispielsweise Abfrageparameter, die eine durch Trennzeichen getrennte Liste der Felder, wie akzeptiert _/orders?fields ProductID, Menge =_.

> [AZURE.TIP] Geben Sie alle optionalen Parameter in der Abfragezeichenfolgen aussagekräftige Standardeinstellungen. Beispielsweise die `limit` 10-Parameter und die `offset` Parameter 0 bei der Implementierung Seitenumbruch, setzen Sie den Sortierparameter auf der Ressource an, wenn Sie die Sortierung implementieren und Festlegen der `fields` Parameter für alle Felder in der Ressource, wenn Sie Projektionen unterstützen.

### <a name="handling-large-binary-resources"></a>Behandeln von großen binäre Ressourcen

Eine einzelne Ressource enthalten möglicherweise große binäre Felder, z. B. Dateien und Bildern. Um die Übertragung zu umgehen Probleme zurückzuführen unzuverlässigen und wiederkehrender Verbindungen und zur Verbesserung der Reaktionszeiten, erwägen Sie die Bereitstellung der Vorgänge, mit die solche Ressourcen, die von der Clientanwendung in Blöcken abgerufen werden können. Hierzu sollte das Web-API unterstützen die Kopfzeile akzeptieren-Bereiche für GET-Anforderung von großen Ressourcen und idealerweise Kopf HTTP-Anfragen für diese Ressourcen implementieren. Die Kopfzeile akzeptieren-Bereiche gibt an, dass der GET-Vorgang Teilergebnisse unterstützt und eine Clientanwendung GET-Anfragen senden kann, die eine Teilmenge einer Ressource festgelegte eines Bereichs von Bytes zurückzugeben. Eine Anforderung Kopf ähnelt eine GET-Anforderung, außer dass sie nur eine Kopfzeile zurückgibt, die der Ressource und einer leeren Nachrichtentext beschreibt. Eine Clientanwendung kann eine Anforderung Kopf zu bestimmen, ob eine Ressource abgerufen werden sollen, mithilfe von teilweise GET-Anfragen ausgeben. Im folgenden Beispiel wird die Anforderung einer Kopf, die Informationen zu einem Produkt Abbild abruft:

```HTTP
HEAD http://adventure-works.com/products/10?fields=productImage HTTP/1.1
...
```

Die Antwortnachricht enthält eine Kopfzeile, die die Größe der Ressource (4580 Byte) enthält, und die Kopfzeile akzeptieren-Bereiche, dass der entsprechende Ladevorgang Ergebnisse teilweise unterstützt:

```HTTP
HTTP/1.1 200 OK
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
...
```

Die Clientanwendung kann diese Informationen verwenden, um eine Reihe von GET-Vorgänge zum Abrufen des Bilds in kleineren Segmenten zu erstellen. Die erste Anforderung abgerufen die ersten 2500 Byte mithilfe der Kopfzeile Bereich:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=0-2499
...
```

Die Antwortnachricht gibt an, dass dies eine teilweise Antwort nach HTTP-Statuscode 206 zurückgeben. Der Content-Length-Header gibt die tatsächliche Anzahl von Bytes, die im Nachrichtentext (nicht die Größe der Ressource) zurückgegeben, und die Kopfzeile des Bereichs Inhalts zeigt an, welcher Teil der Ressource Dies ist (0-2499 out-of 4580 Bytes):

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580
...
_{binary data not shown}_
```

Eine nachfolgende Anforderung von der Clientanwendung abrufen kann den Rest der Ressource mithilfe einer entsprechenden Bereich Kopfzeile:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=2500-
...
```

Die entsprechende Ergebnisnachricht sollte wie folgt aussehen:

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2080
Content-Range: bytes 2500-4580/4580
...
```

## <a name="using-the-hateoas-approach-to-enable-navigation-to-related-resources"></a>Aktivieren der Navigation zu zugehörige Ressourcen mithilfe des HATEOAS Ansatzes

Eines der primären Motive hinter REST ist, dass es den ganzen Satz von Ressourcen zu navigieren, ohne Vorkenntnisse des URI-Schemas möglich sein soll. HTTP GET-Anforderung sollte die Informationen erforderlich sind, um die Ressourcen, die im Zusammenhang mit dem angeforderten Objekt über Links in der Antwort enthalten direkt zu finden, und es auch mit Informationen über die verfügbaren Vorgänge auf jede dieser Ressourcen bereitgestellt werden soll. Dieses Prinzip wird als HATEOAS oder Hypertext-Engine Anwendungsstatus als bezeichnet. Das System effektiv einer begrenzten Zustand Computer, und die Antwort auf jede Anforderung enthält die Informationen, die erforderlich sind, um von einem Zustand in einen anderen wechseln; keine weiteren Informationen sollten vorgenommen werden.

> [AZURE.NOTE] Es sind derzeit keine Standards oder die Spezifikationen, die wie das Prinzip HATEOAS modellieren definieren. Die Beispiele in diesem Abschnitt veranschaulichen eine mögliche Lösung.

Beispielsweise sollte um die Beziehung zwischen Kunden und Bestellungen, verarbeitet die Daten in der Antwort für einen bestimmten Reihenfolge zurückgegeben enthalten URIs in Form eines Hyperlinks identifizieren die Kunden, der die Bestellung, und die Vorgänge, die für diesen Kunden ausgeführt werden können.

```HTTP
GET http://adventure-works.com/orders/3 HTTP/1.1
Accept: application/json
...
```

Hauptteil der Antwortnachricht enthält eine `links` Matrix (im Beispiel hervorgehoben), die die Art der Beziehung (_Kunden_), den URI des Kunden (_http://adventure-works.com/customers/3_), gibt an, wie die Details dieses Kunden (_erhalten_), und die MIME-Typen, die der Webserver Abrufen dieser Informationen (_Text/Xml_ und _Anwendung/Json_) unterstützt abgerufen. Dies ist die Informationen aus, die eine Clientanwendung die Details des Kunden abrufen können muss. Darüber hinaus enthält die Matrix Links auch Links für die anderen Vorgänge, die ausgeführt werden können, wie setzen (zum Ändern des Kunden, zusammen mit dem Format, das der Webserver den Client bereitstellen erwartet), und löschen.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[(some links omitted){"rel":"customer","href":" http://adventure-works.com/customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":"
customer","href":" http://adventure-works.com /customers/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"customer","href":" http://adventure-works.com /customers/3","action":"DELETE","types":[]}]}
```

Vollständigkeit sollte die Matrix Links ebenfalls sind sich selbst verweisende Informationen zu der Ressource, die abgerufen wurde. Diese Links aus dem vorherigen Beispiel weggelassen wurden, aber Sie werden in den folgenden Code hervorgehoben. Beachten Sie, dass diese Hyperlinks, die Beziehung _Self_ verwendet wurde, um darauf hinzuweisen, dass dies ein Verweis auf die Ressource wird durch den Vorgang zurückgegebenen ist:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[{"rel":"self","href":" http://adventure-works.com/orders/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" self","href":" http://adventure-works.com /orders/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"self","href":" http://adventure-works.com /orders/3", "action":"DELETE","types":[]},{"rel":"customer",
"href":" http://adventure-works.com /customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" customer" (customer links omitted)}]}
```

Für diesen Ansatz wirksam werden müssen Sie Clientanwendungen vorbereiten abrufen und diese zusätzlichen Informationen zu analysieren.

## <a name="versioning-a-restful-web-api"></a>Versioning eine Rest Web-API

Es ist sehr wahrscheinlich nicht, die in allen jedoch die einfachste Situationen, dass eine Web-API statisch bleiben. Wie Business Anforderungen neue Sammlungen ändern Ressourcen hinzugefügt werden können, ändert sich möglicherweise die Beziehungen zwischen Ressourcen und die Struktur der Daten in Ressourcen möglicherweise geändert werden. Beim Aktualisieren einer Webs-API, um neue oder unterschiedliche Anforderungen verarbeitet ein relativ einfacher Prozess ist, müssen Sie die Effekte berücksichtigen, die diese Änderungen auf im Web API Verarbeitung Clientanwendungen. Das Problem ist, dass zwar der Entwickler entwerfen und Implementieren eines-API vollständige Kontrolle über diese API hat, der Entwickler nicht den gleichen Grad der Kontrolle über Clientanwendungen die durch Drittanbieter-Organisationen, die per Remotezugriff betrieben erstellt werden kann. Die primäre Notwendigkeit besteht darin, das vorhandene Clientanwendungen weiterhin funktionieren unverändert beizubehalten, wobei neuer Client Applications zu neuen Features und Ressourcen nutzen können.

Versioning ermöglicht eine Web-API an, dass die Features und Ressourcen, die verfügbar gemacht, und eine Clientanwendung kann senden Sie Besprechungsanfragen, die an eine bestimmte Version eines Features oder einer Ressource gerichtet sind. Den folgenden Abschnitten werden die folgenden unterschiedliche Methoden, von denen jede verfügt über eine eigene vor- und Nachteile.

### <a name="no-versioning"></a>Keine Versionskontrolle

Dies ist die einfachste Methode, und kann für einige interne APIs zulässig sein. Große Änderungen könnte als neue Ressourcen oder neue Hyperlinks dargestellt werden.  Hinzufügen von Inhalt zu vorhandenen Ressourcen möglicherweise keine Änderung abgeschnitten werden als Clientanwendungen präsentieren, die nicht angezeigt wird, dass dieses Inhaltstyps einfach ignoriert werden kann erwartet werden.

Beispielsweise sollte eine Anforderung an den URI _http://adventure-works.com/customers/3_ zurückgeben die Details eines einzelnen Kunden mit `id`, `name`, und `address` Felder, die von der Clientanwendung erwartet:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

> [AZURE.NOTE] Aus Gründen der Vereinfachung und Übersichtlichkeit enthalten die Beispiel Antworten in diesem Abschnitt werden nicht HATEOAS Links.

Wenn die `DateCreated` wird das Schema der Ressource Kunden Feld hinzugefügt, und klicken Sie dann die Antwort würde so aussehen:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":"1 Microsoft Way Redmond WA 98053"}
```

Vorhandene Clientanwendungen möglicherweise weiterhin nicht ordnungsgemäß funktioniert, wenn sie nicht erkannten Felder ignorieren sind, während neue Clientanwendungen entworfen werden können, dieses neue Feld verarbeitet werden kann. Jedoch möglicherweise Wenn radikalere Änderungen an das Schema der Ressourcen (z. B. entfernen oder Umbenennen von Feldern) auftreten, oder ändern die Beziehungen zwischen Ressourcen dann diese abgeschnitten werden Änderungen dar, die verhindern, dass vorhandene Clientanwendungen nicht ordnungsgemäß funktioniert. In den folgenden Situationen sollten Sie eine der folgenden Vorgehensweisen.

### <a name="uri-versioning"></a>URI-versioning

Jedes Mal, die Sie im Web API ändern, oder ändern das Schema der Ressourcen, fügen Sie eine Versionsnummer zum URI für jede Ressource. Die bereits vorhandenen URIs sollten weiterhin wie zuvor, bei Ressourcen, die entsprechen den ursprünglichen Schema zurückgeben.

Erweitern im vorherige Beispiel, wenn der `address` Feld ist neu in untergeordnete Felder mit jeder Bestandteil der Adresse strukturiert (z. B. `streetAddress`, `city`, `state`, und `zipCode`), durch einen URI eine Versionsnummer, z. B. http://adventure-works.com/v2/customers/3 mit dieser Version der Ressource ausgesetzt sein:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Dieses Verfahren Versioning ist sehr einfach, doch hängt vom Server die Anfrage an den entsprechenden Endpunkt routing. Allerdings können Sie als im Internet, die über mehrere Iterationen API Laufe der Zeit schwerfällig werden und muss der Server eine Reihe von unterschiedlichen Versionen unterstützen. Darüber hinaus aus einer Purist Perspektive, in allen Fällen die Clientanwendungen sind Abrufen von dieselben Daten (Customer 3), damit der URI nicht wirklich Installationsschritte abhängig davon, die Version werden sollen. Dieses Schema kompliziert auch Implementierung von HATEOAS an, wie die Versionsnummer in ihre URIs enthalten alle Links müssen.

### <a name="query-string-versioning"></a>Abfrage Zeichenfolge versioning

Anstatt bietet mehrere URIs, können Sie die Version der Ressource angeben, mithilfe eines Parameters innerhalb der Abfragezeichenfolge angefügt, um die HTTP-Anforderung, wie z. B. _http://adventure-works.com/customers/3?version=2_. Der Parameter Version sollte einen sinnvollen Wert, z. B. 1 standardmäßig von älteren Clientanwendungen ausgelassen wird.

Dieser Ansatz hat die semantische nutzen, die dieselbe Ressource ist immer aus denselben URI abgerufen werden, aber es hängt von den Code, der verarbeitet die Anforderung zum Analysieren der Abfragezeichenfolge und wieder zu die entsprechende HTTP-Antwort senden. Dieser Ansatz weist auch die gleichen Komplikationen für HATEOAS URI Versioning dafür implementieren.

> [AZURE.NOTE] Einige ältere Webbrowser und Webproxys werden nicht für Besprechungsanfragen Antworten zwischengespeichert werden, die eine Abfragezeichenfolge in der URL enthalten. Dies kann sich unerwünschten auswirken, auf die Leistung für Webanwendungen, die eine Web-API verwenden und die solche in einem Webbrowser heraus ausführen.

### <a name="header-versioning"></a>Kopfzeile versionsverwaltung

Statt die Versionsnummer als Zeichenfolge Abfrageparameter angefügt wird, könnten Sie einen benutzerdefinierten Header implementieren, der die Version der Ressource angibt. Dieser Ansatz ist erforderlich, dass die Clientanwendung alle Besprechungsanfragen, den entsprechenden Header hinzufügt, auch wenn der Code, von die Clientanforderung einen Standardwert (Version 1) verwenden kann, fehlt die Version Kopfzeile. In den folgenden Beispielen nutzen, einen benutzerdefinierten Header mit dem Namen _Benutzerdefinierte-Kopfzeile_. Der Wert dieser Header gibt die Version von Web-API an.

Version 1:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=1
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Version 2:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=2
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Beachten Sie, dass wie bei der vorherigen beiden Methoden implementieren HATEOAS erfordert die entsprechende benutzerdefinierte Kopfzeile in alle Links, einschließlich.

### <a name="media-type-versioning"></a>Geben Sie Versioning Medien

Wenn eine Clientanwendung eine HTTP GET-Anforderung auf einen Webserver sendet sollten sie das Format des Inhalts vorschreiben, die es mithilfe einer Kopfzeile akzeptieren verarbeiten kann, wie zuvor in diesem Handbuch beschrieben. Häufig ist der Zweck der Kopfzeile _annehmen_ dürfen die Clientanwendung, um anzugeben, ob der Hauptteil der Antwort werden sollen, XML, JSON oder einige gebräuchliches Format, das der Client analysiert werden kann. Es ist jedoch möglich, benutzerdefinierte Medientypen zu definieren, die Aktivierung der Clientanwendung, um anzugeben, welche Version einer Ressource erwartet wird, Informationen enthalten. Im folgenden Beispiel wird eine Anforderung, die einer Kopfzeile _akzeptieren_ gibt an, mit dem Wert _application/vnd.adventure-works.v1+json_. Das Element _vnd.adventure-works.v1_ zeigt den Webserver an, dass Wert zurückgegeben werden muss Version 1 der Ressource, während das _Json_ -Element gibt an, dass das Format des Textkörpers Antwort JSON werden soll:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Accept: application/vnd.adventure-works.v1+json
...
```

Der Code, von die Anforderung ist verantwortlich für die Verarbeitung von der Kopfzeile _akzeptieren_ , und es so weit wie möglich (die Clientanwendung möglicherweise mehreren Formaten in der Kopfzeile _akzeptieren_ in der Fall Webserver das am besten geeignete Format für den Textkörper der Antwort auswählen kann angeben) berücksichtigt. Webserver bestätigt das Format der Daten in den Textkörper der Antwort mithilfe der Kopfzeile Inhaltstyp:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Wenn die Kopfzeile akzeptieren keine bekannten Medientypen angegeben wird, konnte den Webserver generieren eine Antwortnachricht HTTP 406 (nicht annehmbar) oder eine Nachricht mit einer standardmäßigen Medientypen zurückzukehren.

Dieser Ansatz ist wohl die reinsten die versionsverwaltung Verfahren und ist natürlich für HATEOAS, die den MIME-Typ von verwandten Daten in der Ressourcenlinks enthalten sein können.

> [AZURE.NOTE] Wenn Sie eine Strategie Versioning auswählen, auch sollten die Auswirkungen auf die Leistung, Sie besonders auf dem Webserver Zwischenspeichern. Die URI Versioning und Abfragezeichenfolge Versioning Phishingversuchen sind Cache geeignete soweit dieselbe Kombination von URI-Abfrage Zeichenfolge mit den gleichen Daten jedes Mal verweist.

> Die Kopfzeile Versioning und Medientypen Versioning Verfahren erfordern in der Regel zusätzliche Logik, um die Werte in der benutzerdefinierte Kopfzeile oder die Kopfzeile akzeptieren überprüfen. In einer umfangreichen-Umgebung können viele Clients mit unterschiedlichen Versionen von einer Web-API sehr viel mehrfach vorhandener Daten in einen serverseitigen Cache führen. Dieses Problem schnell, Spitzen, wenn eine Clientanwendung mit einem Webserver über einen Proxy kommuniziert, implementiert Zwischenspeichern und eine Anforderung an den Webserver, nur weiterleitet, wenn er nicht aktuell eine Kopie der angeforderten Daten in seinem Cache enthält.

## <a name="more-information"></a>Weitere Informationen

- Die [Rest Kochbuch](http://restcookbook.com/) enthält eine Einführung in die Rest-APIs erstellen.
- Die Web- [API Checkliste](https://mathieu.fenniak.net/the-api-checklist/) enthält eine nützliche Liste mit Elementen zu berücksichtigen ist beim Entwerfen und Implementieren eines Web-API.
