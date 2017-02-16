<properties 
    pageTitle="So benutzerabonnement Registrierung und Product delegieren" 
    description="Erfahren Sie, wie Benutzer Registrierung und Product Abonnement an Dritte in Azure-API Management delegieren." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>So benutzerabonnement Registrierung und Product delegieren

Delegierung können Sie Ihre vorhandene Website verwenden, um die Entwicklertools Sign-in/sign-Einrichtung und Produkte im Gegensatz zum Verwenden der integrierten Funktionen in der Entwicklerportal-Abonnement. Dadurch wird Ihrer Website die Benutzerdaten besitzen, und führen Sie die Überprüfung Schritte Benutzerdefiniert.

## <a name="delegate-signin-up"> </a>Delegieren Entwicklertools Anmeldung und Anmeldung

Um die Stellvertretung Entwickler Anmeldung und melden sich mit Ihrer vorhandenen Website müssen Sie einen Endpunkt Delegation Inhalte auf Ihrer Website zu erstellen, der als Einstiegspunkt für ein solcher Antrag initiiert von Entwicklertools API Verwaltungsportal fungiert.

Der Workflow abgeschlossen werden wie folgt:

1. Entwicklertools Klicks auf den Hyperlink der Anmeldung oder Anmeldung bei der API Management-Entwicklerportal
2. Browser wird an den Endpunkt Delegation umgeleitet.
3. Delegation Endpunkt im Gegenzug leitet an oder bietet Benutzeroberfläche Benutzer zur Anmeldung oder Anmeldung auffordert
4. Bei Erfolg wird der Benutzer wieder an die API Management Entwicklertools-Portalseite umgeleitet, die sie aus den ersten


Um zu beginnen, fordert uns erste ansetzen API Management zum Weiterleiten von über Ihre Delegation Endpunkt. Klicken Sie im API Publisher Verwaltungsportal auf **Sicherheit** , und klicken Sie dann auf die Registerkarte **Delegierung** . Klicken Sie auf das Kontrollkästchen, um "Delegate Anmeldung und Anmeldung" zu aktivieren.

![Seite Delegation][api-management-delegation-signin-up]

* Entscheiden Sie, was die URL Ihrer Inhalte Delegation Endpunkt und geben Sie es im Feld **Delegation Endpunkt-URL** ein. 

* Geben Sie in das Feld **Delegation Authentifizierung Key** ein Geheimnis, die zum Berechnen einer Signatur bereitgestellt, um Sie zur Überprüfung, um sicherzustellen, dass die Anforderung tatsächlich um Azure-API Management stammt, verwendet werden. Sie können klicken Sie auf die Schaltfläche **generieren** , damit API Managemnet zufällig einen Schlüssel zu generieren.

Jetzt müssen Sie die **Delegierung Endpunkt**zu erstellen. Es gibt eine Reihe von Aktionen ausführen:

1. Erhalten Sie eine Anforderung in folgender Form:

    > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= {URL der Quellseite} & Salt = {Zeichenfolge} & Sig = {Zeichenfolge}*

    Abfrageparameter für den Fall Anmeldung / Anmeldung:
    - **Vorgang**: identifiziert Delegation in welcher anfordern, es ist – sie können **Anmeldefenster** nur in diesem Fall werden
    - **ReturnUrl**: die URL der Seite, wo der Benutzer geklickt, klicken Sie auf einen Link Anmeldung oder Anmeldung haben,
    - **Salt**: eine spezielle Saltzeichenfolge zum Berechnen eines Hashs Sicherheit verwendet
    - **SIG**: Hashfunktion berechnete Sicherheit, damit Ihre eigenen Vergleich verwendet werden berechneter Hash

2. Stellen Sie sicher, dass die Anfrage aus Azure-API Management (optional, aber dringend empfohlen, für die Sicherheit) stammt

    * Berechnen eines HMAC-SHA512 Hashs einer Zeichenfolge, die auf Grundlage der Abfrageparameter **ReturnUrl** und **salt** ([Beispielcode unten]):
        > HMAC (**salt** + "\n" + **ReturnUrl**)
         
    * Vergleichen Sie den, die von der Abfrageparameter **Sig** Hashwert über berechnet. Wenn die beiden Hashs entsprechen, fahren Sie mit dem nächsten Schritt fort, andernfalls verweigern Sie die Anfrage.

2. Stellen Sie sicher, dass Sie einen Antrag auf Sign in/Anmeldung erhält: der **Vorgang** Abfrage-Parameter auf "**Anmeldefenster**" festgelegt wird.

3. Präsentieren Sie mit UI Anmeldung oder Anmeldung des Benutzers

4. Wenn der Benutzer bei der Anmeldung oben ist müssen Sie ein entsprechendes Konto für diese bei API-Verwaltung erstellen. [Erstellen eines Benutzers] mit der API Management REST-API. Wenn Sie dies ausführen, stellen Sie sicher, dass Sie die Benutzer-ID in denselben, Ihre Benutzer-Store oder auf eine ID, die Sie nachverfolgen können.

5. Wenn der Benutzer erfolgreich authentifiziert wird:

    * über die API Management REST-API [anfordern ein Token einmaliges Anmelden (SSO)]

    * Fügen Sie einen Abfrageparameter ReturnUrl SSO-URL, die Sie aus dem Anruf API über erhalten haben:
        > z. B. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * Leiten Sie den Benutzer zur oben erzeugten URL

Zusätzlich zu den **Anmeldefenster** auswirken können Sie auch Konto Management durch die vorherigen Schritte ausführen, und verwenden eine der folgenden Vorgänge ausführen.

-   **Kennwort ändern**
-   **ChangeProfile**
-   **CloseAccount**

Sie müssen die folgenden Abfrageparameter für Konto Management Vorgänge übergeben.

-   **Vorgang**: welche Art von Delegation Anforderung hat (Kennwort ändern, ChangeProfile oder CloseAccount) bezeichnet
-   **Benutzer-ID**: die Benutzer-Id des Kontos verwalten
-   **Salt**: eine spezielle Saltzeichenfolge zum Berechnen eines Hashs Sicherheit verwendet
-   **SIG**: Hashfunktion berechnete Sicherheit, damit Ihre eigenen Vergleich verwendet werden berechneter Hash

## <a name="delegate-product-subscription"> </a>Delegieren Produkt-Abonnements

Delegieren der Produkt-Abonnement verhält sich über delegierende Benutzer Anmeldung/auszurichten. Der Workflow abgeschlossen würde wie folgt aussehen:

1. Entwicklertools ein Produkt im API Entwicklertools Verwaltungsportal klickt und anschließend auf die Schaltfläche Abonnieren
2. Browser wird an den Endpunkt Delegation umgeleitet.
3. Delegation Endpunkt führt erforderlichen Produkt Abonnement Schritte aus – Dies liegt bei Ihnen und nach sich ziehen kann Umleiten zu einer anderen Seite Zahlungsinformationen zusätzliche Fragen stellen, oder einfach die Informationen zu speichern und keine Anmeldung Eingriff des Benutzers anfordern


Um die Funktionalität zu aktivieren, klicken Sie auf der Seite **Delegation** auf **Stellvertretung Produkt Abonnement**.

Stellen Sie anschließend sicher, dass der Endpunkt Delegation führt die folgenden Aktionen aus:


1. Erhalten Sie eine Anforderung in folgender Form:

    > *http://www.yourwebsite.com/apimdelegation?operation= {Vorgang} & ProductId = {Produkt zum Abonnieren} & Benutzer-ID = {Benutzer Anforderung ausführenden} & salt = {Zeichenfolge} & Sig = {Zeichenfolge}*

    Abfrageparameter für das Produkt Abonnement Groß-/Kleinschreibung:
    - **Vorgang**: identifiziert, welche Art von Delegation Anforderung hat. Produkt-Abonnements sind Anfragen gültigen Optionen:
        - "Abonnieren": eine Anforderung den Benutzer auf ein bestimmtes Produkt mit abonnieren bereitgestellten ID (siehe unten)
        - "Kündigen des Abonnements": eine Anforderung an einen Benutzer aus einem Produkt Kündigen des Abonnements
        - "Erneuern": eine haben ein Abonnement erneuern (z. B., die möglicherweise Ablauf)
    - **ProductId**: die ID des Produkts der Benutzer angefordert abonnieren
    - **Benutzer-ID**: die ID des Benutzers, für wen die angefordert wird,
    - **Salt**: eine spezielle Saltzeichenfolge zum Berechnen eines Hashs Sicherheit verwendet
    - **SIG**: Hashfunktion berechnete Sicherheit, damit Ihre eigenen Vergleich verwendet werden berechneter Hash


2. Stellen Sie sicher, dass die Anfrage aus Azure-API Management (optional, aber dringend empfohlen, für die Sicherheit) stammt

    * Ein HMAC-SHA512 einer Zeichenfolge, die auf Grundlage der **ProductId**, **Benutzer-ID** und **salt** Abfrageparameter zu berechnen:
        > HMAC (**salt** + "\n" + **"ProductID"** + "\n" + **Benutzer-ID**)
         
    * Vergleichen Sie den, die von der Abfrageparameter **Sig** Hashwert über berechnet. Wenn die beiden Hashs entsprechen, fahren Sie mit dem nächsten Schritt fort, andernfalls verweigern Sie die Anfrage.
    
3. Verarbeiten Sie Product Abonnement basierend auf den Typ des Vorgangs in **Vorgang** : z. B. Abrechnung, weiteren Fragen usw. angefordert.

4. Abonnieren Sie Ihr Abonnement erfolgreich des Benutzers mit dem Produkt auf Ihrer Seite aus, klicken Sie auf des Benutzers mit dem Produkt-API Management, indem Sie [Die REST-API für Produkt-Abonnement].

## <a name="delegate-example-code"></a> Beispielcode ##

In diesen Codebeispielen zeigen, wie der *Schlüssel für die Überprüfung Delegation*ausführen, die im Bildschirm Delegation des Publisher-Portals, zum Erstellen eines HMAC die dann zum Überprüfen der Signatur verwendet wird, die Gültigkeit der übergebene ReturnUrl Nachweis festgelegt ist. Derselbe Code funktioniert für "ProductId" und Benutzer-ID mit geringfügigen Änderung.

**C#-Code der ReturnUrl Hash generieren**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS Code der ReturnUrl Hash generieren**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Delegierung finden Sie im folgende Video.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[Anfrage ein Token einmaliges Anmelden (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[Erstellen eines Benutzers]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[die REST-API Aufrufen für Produkt-Abonnement]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[unter bereitgestellten Beispielcode]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 