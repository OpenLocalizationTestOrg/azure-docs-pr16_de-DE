<properties
    pageTitle="Verwenden von Azure CDN mit CORS | Microsoft Azure"
    description="Erfahren Sie, wie die Azure Content Delivery Network (CDN) zu mit Cross-Origin Ressource freigeben (CORS) verwenden."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Verwenden von Azure CDN mit CORS     

## <a name="what-is-cors"></a>Was ist CORS?

CORS (Cross Origin gemeinsames Nutzen von Ressourcen) ist ein HTTP-Feature, mit eine Webanwendung ausgeführt unter einer Domäne auf Ressourcen in einer anderen Domäne zugreifen können. Websiteübergreifende Skriptingtools Angriffen zu reduzieren, implementieren alle modernen Webbrowser eine Einschränkung für die Sicherheit als [same Origin Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy)bezeichnet.  Dadurch wird verhindert, dass eine Webseite von Aufrufen von APIs in eine andere Domäne.  CORS bietet eine sichere Möglichkeit, eine Domäne (die Origin) in eine andere Domäne APIs aufrufen zulassen.
 
## <a name="how-it-works"></a>So funktioniert es
1.  Im Browser sendet die Anforderung Optionen mit einem **Origin** HTTP-Header an. Der Wert der diese Kopfzeile ist die Domäne, die auf die übergeordneten Seite gehostet. Wenn eine Seite aus https://www.contoso.com versucht, die Daten eines Benutzers in der Domäne fabrikam.com zugreifen, würde die folgende Anforderung Kopfzeile fabrikam.com gesendet werden: 
    
    `Origin: https://www.contoso.com`
 
2.  Der Server kann mit den folgenden Antworten:
    - Eine **Access Steuerelement zulassen Origin** Kopfzeile in der Antwort, die angibt, welche Websites Origin zulässig sind. Beispiel:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Eine Fehlerseite, wenn der Server nicht die Anfrage Cross-Origin zulässt
    - Eine **Access Steuerelement zulassen Origin** Kopfzeile mit einem Platzhalter, der alle Domänen ermöglicht:
        
        `Access-Control-Allow-Origin: *`
 
Für komplexe HTTP-Anfragen es gibt eine "preflight" Anforderung fertig ersten feststellen, ob sie vor dem Senden der gesamten Anforderung Berechtigung verfügt.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Platzhalter oder einzelne Origin Szenarien

CORS auf Azure CDN funktionieren automatisch ohne zusätzliche Konfiguration, wenn die Kopfzeile **Access Steuerelement zulassen Origin** Platzhalterzeichen (*) oder ein einzelnes Origin festgelegt ist.  Das CDN wird die erste Antwort zwischenspeichern und nachfolgende Anforderungen werden die gleiche Kopfzeile verwenden.
 
Wenn bereits Anfragen zu dem CDN vor CORS festzulegenden auf vorgenommen wurden die Ihrer Origin, müssen Sie Bereinigen von Inhalten auf Ihre Inhalte Endpunkt, um den Inhalt mit der Kopfzeile **Access Steuerelement zulassen Origin** neu zu laden.
 
## <a name="multiple-origin-scenarios"></a>Szenarien mit mehreren origin

Wenn Sie zulassen eine bestimmte Liste von Ursprung für CORS zugelassen werden müssen, erhalten die Dinge etwas komplizierter. Das Problem tritt auf, wenn es sich bei das CDN die Kopfzeile **Access Steuerelement zulassen Origin** für den ersten CORS Ursprung speichert.  Wenn Sie ein anderen CORS Ursprung nachfolgend anfordert, wird das CDN served Cache **Access Steuerelement zulassen Origin** Kopfzeile wird nicht entsprechen.  Es gibt mehrere Methoden, um dies zu beheben.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium aus Verizon

So aktivieren Sie dies am besten **Azure CDN Premium von Verizon**, mit denen einige erweiterte Funktionen zur Verfügung stellt. 
 
Zum [Erstellen einer Regel](cdn-rules-engine.md) müssen die Kopfzeile **Ursprung** der Anforderung zu überprüfen.  Wenn es sich um eine gültige Origin ist, wird die Regel die Kopfzeile **Access Steuerelement zulassen Origin** mit den Ursprung in der Anforderung bereitgestellten festgelegt.  Wenn Sie der Ursprung in der Kopfzeile **Origin** angegeben ist nicht zulässig, sollte die Regel die Kopfzeile **Access Steuerelement zulassen Origin** unterdrückt, die was im Browser ablehnen verursacht. 
 
Es gibt zwei Möglichkeiten, mit der Regeln-Engine Aktion aus.  In beiden Fällen die **Zugriff Steuerelement zulassen Origin** Kopfzeile von der Dateiserver Origin wird vollständig ignoriert, die CDNs Regeln-Engine verwaltet vollständig die zulässige CORS Ursprung.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Eine reguläre Ausdrücke mit beliebigen gültigen Ursprung
 
In diesem Fall erstellen Sie eine reguläre Ausdrücke, die alle den Ursprung enthält, die Sie zulassen möchten: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure CDN von Verizon** wird [Perl kompatible reguläre Ausdrücke](http://pcre.org/) als deren-Engine für reguläre Ausdrücke verwendet.  Ein Tool wie [reguläre Ausdrücke 101](https://regex101.com/) können Sie um Ihre reguläre Ausdrücke zu überprüfen.  Beachten Sie, dass das Zeichen "/" reguläre Ausdrücke gültig ist und nicht mit Escapezeichen versehen werden muss, allerdings Escapezeichen dieses Zeichen und wird als bewährte Methode betrachtet wird von einigen Regex Validatoren erwartet.

Wenn der reguläre Ausdruck entspricht, ersetzt die Regel **Access Steuerelement zulassen Origin** Kopfzeile (falls vorhanden) vom Ursprung mit den Ursprung, die die Anforderung gesendet.  Sie können auch zusätzliche CORS Header, z. B. **Access Steuerelement zulassen Methoden**hinzufügen.

![Beispiel für Regeln mit reguläre Ausdrücke](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Kopfzeile-Regel für jede Origin anfordern.

Anstatt reguläre Ausdrücke können Sie stattdessen eine gesonderte Regel für jeden Origin, dass Sie mit der [Bedingung entsprechen](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1) **Anfordern Kopfzeile Platzhalterzeichen** zulassen möchten erstellen. Wie die Methode für reguläre Ausdrücke, setzt die Regeln-Engine allein die Kopfzeilen CORS. 
  
![Regeln Beispiel ohne reguläre Ausdrücke](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] Im Beispiel oben, die Verwendung von Platzhalterzeichen * die Regeln-Engine HTTP- und HTTPS entsprechend zu erfahren.
 
### <a name="azure-cdn-standard"></a>Azure CDN Standard

Auf Azure CDN Standard Profile sollten die einzige Verfahren für mehrere Ursprung ohne Verwendung von Platzhalterzeichen Ursprung dürfen [Abfrage-Zeichenfolge Cache](cdn-query-string.md)verwenden.  Sie müssen die Zeichenfolge-Einstellung einer Abfrage für den Endpunkt CDN aktivieren und verwenden Sie dann eine eindeutige Abfragezeichenfolge für jede zulässige Domäne-Anfragen. Auf diese Weise führt die CDN ein anderes Objekt für jede eindeutige Abfragezeichenfolge Zwischenspeichern. Dieser Ansatz ist jedoch nicht ideal, da es mehrere Kopien derselben Datei auf das CDN zwischengespeichert wird.  

