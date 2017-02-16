<properties
   pageTitle="Konfigurierbare Token Lebensdauer in Azure-Active Directory | Microsoft Azure"
   description="Dieses Feature wird von Administratoren und Entwickler verwendet, um anzugeben, die Gültigkeitsdauer des Token ausgestellt von Azure AD."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/06/2016"
   ms.author="billmath"/>


# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Konfigurierbare Token Lebensdauer in Azure-Active Directory (Public Preview)

>[AZURE.NOTE]
>Diese Funktion ist derzeit in public Preview-Version.  Sie sollten zum Zurücksetzen oder löschen alle Änderungen bereit sein.  Wir werden dieses Feature für alle Benutzer, die Sie bei der öffentlichen Vorschau probieren Öffnung, jedoch möglicherweise bestimmte Aspekte ein [Azure AD Premium-Abonnement](active-directory-get-started-premium.md) , sobald allgemein verfügbar erforderlich.


## <a name="introduction"></a>Einführung
Dieses Feature wird von Administratoren und Entwickler verwendet, um anzugeben, die Gültigkeitsdauer des Token ausgestellt von Azure AD. Token Lebensdauer können für alle apps in einen Mandanten, für eine Anwendung mit mehreren Mandanten oder für einen bestimmten Dienst Hauptbenutzer in einen Mandanten konfiguriert werden.

Im Azure AD stellt Sie ein Gruppenrichtlinienobjekt eine Reihe von Regeln für einzelne Applikationen oder alle Anwendungen in einen Mandanten erzwungen.  Jeder Richtlinie verfügt über eine eigene Struktur mit einer Reihe von Eigenschaften, die klicken Sie dann auf Objekte angewendet werden, die sie zugeordnet sind.

Eine Richtlinie kann für einen Mandanten als Standard festgelegt werden. Dieser Richtlinie wirksam klicken Sie dann auf eine beliebige Anwendung, die in die Mandanten, solange sie nicht durch eine Richtlinie mit höherer Priorität überschrieben wird. Richtlinien können auch bestimmte Applikationen zugewiesen werden. Die Reihenfolge der Priorität variieren je nach Richtlinientyp.

Token Lebensdauerrichtlinien können für Token aktualisieren, Access Token, Sitzungstoken und ID Token konfiguriert werden.


### <a name="access-tokens"></a>Access-Token
Ein Access-Token wird durch einen Client verwendet, auf eine geschützte Ressource zuzugreifen. Eine Access-Token kann nur für eine bestimmte Kombination von Benutzer, Client und Ressourcen verwendet werden. Access-Token können nicht aufgehoben werden sollen und bis zu deren Ablauf gültig sind. Ein bösartiger Akteur, der eine Access-Token erhalten hat können sie für Handlungen ihres Ablaufdatums verwenden.  Anpassen des Sicherheitstokens Access-Lebensdauer ist eine Wechselwirkung zwischen bessere Systemleistung und den Zeitraum, dass der Client Access behält, nachdem das Konto des Benutzers deaktiviert ist.  Verbesserung der Systemleistung wird durch Verringern der Anzahl, wie oft, die ein Client ein Token frisch Access abrufen muss, erreicht. 

### <a name="refresh-tokens"></a>Aktualisieren von Token
Wenn ein Client eine Access-Token Zugriff auf eine geschützte Ressource erhält, empfängt es an einem Token aktualisieren und eine Access-Token. Das Aktualisieren Token wird verwendet, um die neue Access/aktualisieren token Paare zu erhalten, wenn die aktuelle Access-Sicherheitstoken abläuft. Aktualisieren von Token an Kombinationen aus Benutzer und Client gebunden sind. Sie können aufgehoben werden sollen, und deren Gültigkeit aktiviert ist, jedes Mal, wenn sie verwendet werden.

Es ist wichtig, unterschieden zwischen vertraulichen und öffentliche Kunden. Vertrauliche Clients sind Anwendungen, die ein Clientkennwort sicher speichern können sind so, dass sie um nachzuweisen, dass Anfragen von der Clientanwendung und keine bösartige Akteur stammen. Wie diese Zahlungen sicherer, die Standardwerte für die Gültigkeitsdauer Aktualisierungsvorgangs Token ausgestellt für diese Zahlungen sind höher und können nicht geändert werden, verwenden die Richtlinie.

Öffentliche Clients erfolgreiche aufgrund der Umgebung, die die Programme ausgeführt, in werden Schwächen, ein Clientkennwort sicher zu speichern. Richtlinien können auf Ressourcen verhindern, dass die Aktualisierung Token aus öffentlichen Clients, die älter als eine bestimmte Periode zurück erhalten ein neues Access/aktualisieren token Paar (Aktualisieren Token inaktive Max-Zeit) festgelegt werden.  Darüber hinaus können Richtlinien verwendet werden, um einen Zeitraum festzulegen, jenseits derer die Aktualisierung nicht mehr Token sind (Token Max Age aktualisieren) akzeptiert.  Anpassen des aktualisieren token Lebensdauer können Sie steuern, wann und wie oft der Benutzer Anmeldeinformationen im Hintergrund erneut authentifiziert wird, wenn mithilfe einer öffentlichen Clientanwendung erneut erforderlich ist.


### <a name="id-tokens"></a>ID-Token
ID-Token werden auf Websites und systemeigenen Clients übergeben und Profilinformationen über einen Benutzer enthalten. Eine Token-ID, die auf eine bestimmte Kombination von Benutzer- und Client gebunden ist. ID-Token werden bis Ablauf als gültig angesehen.  Normalerweise entspricht eine Webanwendung ein Benutzers der Sitzung Lebensdauer in der Anwendung, um die Lebensdauer der Token-ID für den Benutzer ausgestellt.  Anpassen des Sicherheitstokens ID-Lebensdauer können Sie steuern, wie oft die Anwendung die Anwendung Sitzung abläuft und erfordern, dass den Benutzer mit Azure AD (im Hintergrund oder interaktiv) erneut authentifiziert werden.

### <a name="single-sign-on-session-token"></a>Einzelne Sitzung anmelden token
Wenn die Benutzerauthentifizierung mit Azure AD wird eine einzelne Sitzung anmelden mit Browser und Azure AD-des Benutzers hergestellt.  Die einzelnen anmelden Sitzung Token, in der Form eines Cookies, steht für diese Sitzung. Es ist wichtig, beachten Sie, dass das Token für einmaliges Anmelden Sitzung nicht an eine bestimmte Ressource-Client-Anwendung gebunden ist. SSO-Sitzungstoken gesperrt werden können, und deren Gültigkeit aktiviert ist, jedes Mal, wenn sie verwendet werden.

Es gibt zwei Arten von SSO-Sitzungstoken. Beständiger Sitzungstoken werden gespeichert, wie dauerhafte Cookies vom Browser und nicht beständige Sitzungstoken als Sitzungscookies gespeichert werden (diese werden gelöscht, wenn der Browser geschlossen wird).

Nicht-beständiger Sitzungstoken ausgestattet eine Lebensdauer von 24 Stunden, während beständige Token eine Lebensdauer von 180 Tagen verfügen. Jedes Mal, wenn das Token für einmaliges Anmelden Sitzung innerhalb der Gültigkeitsdauer verwendet wird, wird die Gültigkeitsdauer einer anderen 24 Stunden oder 180 Tage erweitert. Wenn die SSO-Sitzung, die Token nicht innerhalb der Gültigkeitsdauer verwendet wird davon ausgegangen ist abgelaufen, und werden nicht mehr akzeptiert. 

Richtlinien können verwendet werden, zum Festlegen einer bestimmten Zeitspanne nach die erste Sitzung Token ausgestellt wurde jenseits derer die Sitzungstoken nicht mehr sind (Sitzung Token Max Age) akzeptiert.  Anpassen des Sicherheitstokens Gültigkeitsdauer der Sitzung können Sie steuern, wann und wie oft der Benutzer muss Anmeldeinformationen im Hintergrund Authentifizierung bei Verwendung einer Web-Anwendungs erneut eingeben.

### <a name="token-lifetime-policy-properties"></a>Token Lebensdauer Richtlinieneigenschaften
Eine Richtlinie token Lebensdauer ist ein Gruppenrichtlinienobjekt, die Gültigkeitsdauer token Regeln enthält.  Die Eigenschaften der Richtlinie dienen zur angegebenen token Lebensdauer Steuerung.  Wenn keine Richtlinie festgelegt ist, erzwingt das System den Standardwert für die Gültigkeitsdauer.


### <a name="configurable-token-lifetime-properties"></a>Konfigurierbare token Lebensdauer Eigenschaften
Eigenschaft|Die Eigenschaftenzeichenfolge Richtlinie|Wirkt sich auf|Standard|Minimalwert|Maximum|
----- | ----- | ----- |----- | ----- | ----- |
Access Token Lebensdauer|  AccessTokenLifetime|Zugriff auf Token, ID Token, SAML2 Token|1 Stunde|10 Minuten|1 Tag|
Inaktive Zeit Token Max aktualisieren|    MaxInactiveTime |Aktualisieren von Token |14 Tage|10 Minuten|    90 Tage|
Einfache Aktualisierung Token Max ALTER|    MaxAgeSingleFactor  |Aktualisieren von Token (für alle Benutzer) |90 Tage|10 Minuten |Bis zu widerrufen *|
Mehrstufige aktualisieren Token Max ALTER| MaxAgeMultiFactor|  Aktualisieren von Token (für alle Benutzer)| 90 Tage|10 Minuten|Bis zu widerrufen *|
Einfache Sitzung Token Max ALTER |MaxAgeSessionSingleFactor **    |Sitzungstoken (beständigen und nicht beständige)| Bis zu widerrufen   |10 Minuten |Bis zu widerrufen *|
Mehrstufige Sitzung Token Max ALTER| MaxAgeSessionMultiFactor ***|    Sitzungstoken (beständigen und nicht beständige)| Bis zu widerrufen|  10 Minuten| Bis zu widerrufen *



- * 365 Tage ist die explizite Maximallänge, die für diese Attribute festgelegt werden kann.
- ** Wenn MaxAgeSessionSingleFactor nicht festgelegt ist, nimmt diesen Wert den Wert MaxAgeSingleFactor. Wenn Sie keinen der Parameter festgelegt ist, hat die Eigenschaft auf den Standardwert (bis-widerrufen).
- Wenn MaxAgeSessionMultiFactor nicht festgelegt ist, nimmt diesen Wert den Wert MaxAgeMultiFactor. Wenn Sie keinen der Parameter festgelegt ist, hat die Eigenschaft auf den Standardwert (bis-widerrufen).

### <a name="exceptions"></a>Ausnahmen
Eigenschaft|Wirkt sich auf|Standard|
----- | ----- | ----- |
Aktualisieren von Token Max inaktive Zeit (partnerverbundkontakte Benutzer mit nicht genügend Widerrufsinformationen)|Aktualisieren von Token (ausgestellt für Benutzer mit nicht genügend Widerrufsinformationen im Verbund)|12 Stunden|
Aktualisieren von Token Max inaktive Zeit (vertrauliche Clients)| Aktualisieren von Token (ausgestellt für vertrauliche Clients)|90 Tage|
Aktualisieren von token Max Age (ausgestellt für vertrauliche Clients) |   Aktualisieren von Token (ausgestellt für vertrauliche Clients) |Bis zu widerrufen

### <a name="priority-and-evaluation-of-policies"></a>Priorität und Auswertung von Richtlinien

Token Lebensdauerrichtlinien können erstellt und bestimmte Applikationen, Mandanten und Dienst Hauptbenutzer zugewiesen werden. Dies bedeutet, dass es möglich, dass mehrere Richtlinien für eine bestimmte Anwendung angewendet wird. Die Richtlinien für die Token Lebensdauer, die auch wirklich übernommen wird: Diese Regeln


- Wenn der Dienst Tilgungsanteile explizit eine Richtlinie zugewiesen ist, wird es erzwungen. 
- Wenn keine Richtlinie zu dem Dienst Tilgungsanteile explizit zugewiesen ist, ist eine Richtlinie zu den übergeordneten Mandanten des Diensts Hauptbenutzer explizit zugewiesen werden erzwungen. 
- Wenn keine Richtlinie zu den wichtigsten Dienst oder den Mandanten explizit zugewiesen ist, ist die Richtlinie zur Anwendung zugewiesen werden erzwungen. 
- Wenn keine Richtlinie der Tilgungsanteile Dienst, den Mandanten oder das Anwendungsobjekt zugewiesen wurde, werden die Standardwerte erzwungen werden (siehe Tabelle oben).

Weitere Informationen zu der Beziehung zwischen Anwendung und Dienst Hauptbenutzer Objekten in Azure AD finden Sie unter [Anwendung und Dienst wichtigsten Objekte in Azure Active Directory](active-directory-application-objects.md).

Ein Token Gültigkeit ist gleichzeitig ausgewertet, wenn er verwendet wird. Wirkt sich die Richtlinie mit der höchsten Priorität in der Anwendung, die zugegriffen wird.


>[AZURE.NOTE]
>Beispiel
>
>Ein Benutzer möchte 2 Webanwendungen, A und b zugreifen. 
>
>
>- Beide Programme sind in der gleichen übergeordneten Mandanten. 
>- Token Lebensdauer Richtlinie 1 mit einer Sitzung Token Max Age von 8 Stunden wird als des übergeordneten Mandanten Standard festgelegt.
>- Webanwendung A ist eine Anwendung reguläre verwenden und ist nicht für Richtlinien verknüpft. 
>- Webanwendung B für vertrauliche Prozesse verwendet wird und deren Dienst Tilgungsanteile token Richtlinien für die Gültigkeitsdauer 2 mit einer Sitzung Token Max Age von 30 Minuten verknüpft ist.
>
>12:00 Uhr der Benutzer eine neue Browsersitzung angezeigt wird und versucht, Webanwendung a Zugriff der Benutzer auf Azure AD umgeleitet, und dazu aufgefordert wird, melden Sie sich in. Auf diese Weise ein Cookie mit einem Sitzungstoken im Browser gelöscht. Der Benutzer ist umgeleitet, wechseln Sie wieder zur Anwendung A mit einem ID-Token web, mit dem sie auf die Anwendung zugreifen können.
>
>Bei 12:15 PM versucht, der Benutzer klicken Sie dann auf Webanwendung b zuzugreifen Im Browser leitet zur Azure AD die Sitzung Cookies erkennt. Web Anwendung B Dienst Tilgungsanteile mit einer Richtlinie 1 verknüpft ist, aber auch Teil der übergeordneten-Mandanten mit Standardrichtlinie 2 ist. Richtlinie 2 wird wirksam, da Richtlinien zum Dienst Hauptbenutzer verknüpft eine höhere Priorität als Standardrichtlinien Mandanten haben. Das Sitzungstoken wurde ursprünglich innerhalb der letzten 30 Minuten ausgestellt, damit es gültig ist. Der Benutzer wird mit ihnen Zugriff gewähren ID Token zurück zur Web-Anwendung B umgeleitet.
>
>1:00 Uhr versucht der Benutzer navigieren zu Web-Anwendung A. Der Benutzer wird auf Azure AD umgeleitet. Webanwendung A ist nicht für Richtlinien verknüpft, aber da es einen Mandanten mit Standardrichtlinie 1 ist, wird dieser Richtlinie wirksam. Die Sitzung, die Cookie erkannt wird, die in den letzten 8 Stunden und der Benutzer ursprünglich ausgegeben wurde wird im Hintergrund wieder zu Webanwendung A mit einem neuen ID Token umgeleitet, ohne authentifiziert.
>
>Der Benutzer versucht sofort Zugriff auf Webanwendung B. Der Benutzer wird auf Azure AD umgeleitet. Während der Richtlinie 2 zuvor wirksam wird. Wie das Token mehr als 30 Minuten vor ausgestellt wurde, der Benutzer wird dann aufgefordert, ihre Anmeldeinformationen erneut eingeben, und einer ganz neuen Sitzung und -ID-Token ausgestellt wurde. Der Benutzer kann dann Webanwendung b zugreifen.

## <a name="configurable-policy-properties-in-depth"></a>Eigenschaften der konfigurierbare Richtlinie: greifende

### <a name="access-token-lifetime"></a>Access token Lebensdauer

**Zeichenfolge:** AccessTokenLifetime

**Wirkt sich auf:** Access Token, ID-Token

**Zusammenfassung:** Diese Richtlinie steuert, wie lange Zugriff und ID-Token für diese Ressource gültig sind. Reduzieren der Access-token Gültigkeitsdauer reduziert das Risiko Access- oder Token-ID, die vom verwendeten eines bösartigen Akteurs für eine längere Zeit (wie er nicht gesperrt werden können), aber wirkt sich auch negativ auf die Leistung, wie die Token werden häufiger ersetzt werden müssen.

### <a name="refresh-token-max-inactive-time"></a>Aktualisieren von token maximal inaktive Zeit

**Zeichenfolge:** MaxInactiveTime

**Wirkt sich auf:** Aktualisieren von Token

**Zusammenfassung:** Diese Richtlinie steuert, wie ALT ein Token aktualisieren sein kann, bevor ein Client nicht mehr in ein neues Access/aktualisieren Paar token abrufen, wenn Sie versuchen, den Zugriff auf diese Ressource verwenden kann. Da in der Regel ein neues aktualisieren Token zurückgegeben wird, dass ein Token aktualisieren verwendet wird, muss der Client nicht erreicht haben auf eine beliebige Ressource mit den aktuellen aktualisieren Token für die angegebene Zeitspanne vor Zugriff zu dieser Richtlinie verhindern möchten. 

Dieser Richtlinie zwingt Benutzer, die nicht auf deren Client erneut authentifizieren, um ein neues aktualisieren Token abrufen aktiv wurden. 

Es ist wichtig, beachten Sie, dass die Aktualisierung Token Max inaktive Zeit Wert die einfache Token Max Age und mehrstufige aktualisieren Token Max Age niedriger festgelegt werden müssen.

### <a name="single-factor-refresh-token-max-age"></a>Einfache Aktualisierung token max ALTER

**Zeichenfolge:** MaxAgeSingleFactor

**Wirkt sich auf:** Aktualisieren von Token

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann ein Benutzer aktualisieren Token verwenden, um neue Access/aktualisieren abzurufen token Paare nach dem letzten Mal, das Sie authentifiziert, erfolgreich mit nur einem Einfaktorielle Varianzanalyse weiterhin. Nachdem ein Benutzer authentifiziert und ein neues aktualisieren Token empfängt, werden diese Aktualisierung token illustrieren verwenden (solange das aktuelle aktualisieren Token nicht gesperrt ist und nicht länger als die inaktive Zeit nicht verwendete Links) sein für den angegebenen Zeitraum. An diesem Punkt werden Benutzer zwangsweise erneut authentifizieren, um ein neues aktualisieren Token zu erhalten. 

Verringern das max Alter zwingt Benutzer häufiger authentifizieren. Da einfache Authentifizierung als weniger als eine kombinierte Authentifizierung sicher angesehen wird, empfiehlt es sich, dass diese Richtlinie auf einen Wert gleich oder kleiner als die kombinierte aktualisieren Token Max Alter Richtlinie festgelegt ist.

### <a name="multi-factor-refresh-token-max-age"></a>Mehrstufige aktualisieren token max ALTER

**Zeichenfolge:** MaxAgeMultiFactor

**Wirkt sich auf:** Aktualisieren von Token

**Zusammenfassung:** Aktualisieren Token verwenden, um neue Access/aktualisieren abzurufen token Paare nach dem letzten Mal, das Sie authentifiziert, erfolgreich mit mehreren Faktoren kann diese Richtlinie steuert, wie lange ein Benutzer weiterhin. Nachdem ein Benutzer authentifiziert und ein neues aktualisieren Token empfängt, werden diese Aktualisierung token illustrieren verwenden (solange das aktuelle aktualisieren Token nicht gesperrt ist und nicht länger als die inaktive Zeit nicht verwendete Links) sein für den angegebenen Zeitraum. An diesem Punkt werden Benutzer zwangsweise erneut authentifizieren, um ein neues aktualisieren Token zu erhalten. 

Verringern das max Alter zwingt Benutzer häufiger authentifizieren. Da einfache Authentifizierung als weniger als eine kombinierte Authentifizierung sicher angesehen wird, empfiehlt es sich, dass diese Richtlinie auf einen Wert gleich oder größer als die einfache Aktualisierung Token Max Alter Richtlinie festgelegt ist.

### <a name="single-factor-session-token-max-age"></a>Einfache Sitzung token max ALTER

**Zeichenfolge:** MaxAgeSessionSingleFactor

**Wirkt sich auf:** Sitzungstoken (beständigen und nicht beständige)

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann weiterhin ein Benutzer Sitzungstoken verwenden, können Sie um neue-ID und Sitzung Token nach der letzten Zeit zu gelangen, die sie erfolgreich mit nur einem Einfaktorielle Varianzanalyse authentifiziert. Nachdem ein Benutzer authentifiziert und eine neue Sitzungstoken erhält, werden sie Sitzung token illustrieren verwenden (solange das aktuelle Sitzungstoken nicht gesperrt oder abgelaufen ist) für den angegebenen Zeitraum. An diesem Punkt werden Benutzer zwangsweise erneut authentifizieren, um eine neue Sitzungstoken zu erhalten. 

Verringern das max Alter zwingt Benutzer häufiger authentifizieren. Da einfache Authentifizierung als weniger als eine kombinierte Authentifizierung sicher angesehen wird, empfiehlt es sich, dass diese Richtlinie auf einen Wert gleich oder kleiner als die kombinierte Sitzung Token Max Alter Richtlinie festgelegt ist.

### <a name="multi-factor-session-token-max-age"></a>Mehrstufige Sitzung token max ALTER

**Zeichenfolge:** MaxAgeSessionMultiFactor

**Wirkt sich auf:** Sitzungstoken (beständigen und nicht beständige)

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann weiterhin ein Benutzer Sitzungstoken verwenden, können Sie um neue-ID und Sitzung Token nach der letzten Zeit zu gelangen, die sie erfolgreich mit mehreren Faktoren authentifiziert. Nachdem ein Benutzer authentifiziert und eine neue Sitzungstoken erhält, werden sie den Sitzung token Fluss verwenden (solange das aktuelle Sitzungstoken nicht gesperrt oder abgelaufen ist) für die angegebene Zeitspanne. An diesem Punkt werden Benutzer zwangsweise erneut authentifizieren, um eine neue Sitzungstoken zu erhalten. 

Verringern das max Alter zwingt Benutzer häufiger authentifizieren. Da einfache Authentifizierung als weniger als eine kombinierte Authentifizierung sicher angesehen wird, empfiehlt es sich, dass diese Richtlinie auf einen Wert gleich oder größer als die einfache Sitzung Token Max Alter Richtlinie festgelegt ist.

## <a name="sample-token-lifetime-policies"></a>Beispiel für token Lebensdauerrichtlinien

Zum Erstellen und Verwalten von token Lebensdauer für apps, Dienst Hauptbenutzer und der gesamten Mandanten macht alle Arten von neuen Szenarien in Azure AD möglich.  Wir werden einige häufige Szenarien der Richtlinie durchzuführen, mit denen Sie neue Regeln für die vorgangseinschränkung:


- Token Lebensdauer
- Inaktive Token Max-Zeiten
- Token Max ALTER

Wir werden einige Szenarien einschließlich durchzuführen:


- Verwalten von einem Mandanten Standardrichtlinie
- Erstellen einer Richtlinie für Web anmelden
- Erstellen einer Richtlinie für systemeigene Apps Aufrufen eine Web-API
- Verwalten von Richtlinie Erweitert 

### <a name="prerequisites"></a>Erforderliche Komponenten
In der Stichprobe Szenarien erhalten wir werden erstellen, aktualisieren, verknüpfen und Löschen von Richtlinien auf apps, Dienst Hauptbenutzer und der gesamten Mandanten.  Wenn Sie neu bei Azure AD, Auschecken [in diesem Artikel](active-directory-howto-tenant.md) sind, bevor Sie mit diesen Beispielen behilflich helfen.  


1. Um zu beginnen, laden Sie die neuesten [Azure AD-PowerShell-Cmdlet Vorschau](https://www.powershellgallery.com/packages/AzureADPreview). 
2.  Nachdem Sie die Azure AD-PowerShell-Cmdlets haben, führen Sie verbinden-Befehl melden Sie sich bei Ihrem Konto Azure AD-Admin. Sie müssen dieses Verfahren bei jedem einer neuen Sitzung starten.
        
        Connect-AzureAD -Confirm

3.  Führen Sie den folgenden Befehl aus, um alle Richtlinien anzuzeigen, die in Ihrem Mandanten erstellt wurden.  Dieser Befehl sollte nach die meisten Vorgänge in den folgenden Szenarien verwendet werden.  Es hilft Ihnen außerdem die **Objekt-ID** Ihre Richtlinien abrufen. 
        
        Get-AzureADPolicy

### <a name="sample-managing-a-tenants-default-policy"></a>Beispiel: Verwalten des Mandanten Standardrichtlinie

In diesem Beispiel erstellen wir eine Richtlinie, die den Benutzern ermöglicht, die weniger häufig über Ihre gesamte Mandanten anmelden. 

Dazu erstellen wir eine Richtlinie token Lebensdauer für einfache Aktualisierung Token, die in Ihrem Mandanten angewendet wird. Dieser Richtlinie wird jede Anwendung in Ihrem Mandanten und jeden Dienst Hauptbenutzer, die bereits eine Richtlinie darauf nicht angewendet. 

1.  **Erstellen einer Richtlinie Token Lebensdauer.** 

Legen Sie die einfache Aktualisierung Token auf "bis-widerrufen" Dies bedeutet, dass es wird nicht ablaufen, bis Access gesperrt ist.  Die Richtliniendefinition unten ist, was wir erstellen werden:
        
        @("{
          `"TokenLifetimePolicy`":
              {
                 `"Version`":1, 
                 `"MaxAgeSingleFactor`":`"until-revoked`"
              }
        }")

Führen Sie dann den folgenden Befehl aus, um diese Richtlinie zu erstellen. 

        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1, `"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName TenantDefaultPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
        
Um Ihre neue Richtlinie und ObjectID erhalten, führen Sie den folgenden Befehl ein.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **Aktualisieren Sie die Richtlinie**

Sie haben entschieden, dass die erste Richtlinie nicht ganz strenge ist wie der Dienst erfordert, und haben entschieden, dass Sie einfache Aktualisierung von Token in 2 Tagen ablaufen soll. Führen Sie den folgenden Befehl ein. 
        
    Set-AzureADPolicy -ObjectId <ObjectID FROM GET COMMAND> -DisplayName TenantDefaultPolicyUpdatedScenario -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"2.00:00:00`"}}")
        
&nbsp;&nbsp;3. **war's schon!** 

### <a name="sample-creating-a-policy-for-web-sign-in"></a>Beispiel: Erstellen einer Richtlinie für Web anmelden

In diesem Beispiel erstellen wir eine Richtlinie, die Ihre Benutzer authentifiziert häufiger an Ihre Web App benötigen werden. Diese Richtlinie wird die Lebensdauer von der Access-Id-Token und die Max Age der eine kombinierte Sitzung Token auf die Dienst Tilgungsanteile der Web app festgelegt.

1.  **Erstellen einer Richtlinie Token Lebensdauer.**

Diese Richtlinie für Web anmelden wird die Gültigkeitsdauer Token Access-Id und das Max einfache Sitzung Token Alter auf 2 Stunden festgelegt.

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"AccessTokenLifetime`":`"02:00:00`",`"MaxAgeSessionSingleFactor`":`"02:00:00`"}}") -DisplayName WebPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy

Um Ihre neue Richtlinie und ObjectID erhalten, führen Sie den folgenden Befehl ein.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **weisen Sie die Richtlinie zu Ihrem Dienst Tilgungsanteile.**

Wir werden diese neue Richtlinie mit einem Dienst Tilgungsanteile verknüpfen.  Sie benötigen außerdem eine Möglichkeit zum **ObjectId** von Ihrem Dienst Hauptbenutzer zugreifen. Die [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) -Abfragen oder wechseln Sie zu unseren [Graph-Explorer-Tools](https://graphexplorer.cloudapp.net/) , und melden Sie sich bei Ihrem Konto Azure AD-in Ihrem Mandanten Dienst Hauptbenutzer finden Sie unter. 

Nachdem Sie die **ObjectId**haben, führen Sie den folgenden Befehl ein.
        
    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
&nbsp;&nbsp;3. **war's schon!** 

 

### <a name="sample-creating-a-policy-for-native-apps-calling-a-web-api"></a>Beispiel: Erstellen einer Richtlinie für systemeigene apps Aufrufen eine Web-API

>[AZURE.NOTE]
>Verknüpfen von Richtlinien mit Applications ist derzeit deaktiviert.  Wir arbeiten auf diesem in Kürze aktivieren.  Diese Seite wird aktualisiert, sobald die Funktion verfügbar ist.

In diesem Beispiel werden erstellt eine Richtlinie, die Benutzern die kleiner Authentifizierung erforderlich ist und wird zu den Zeitraum an, die sie inaktive werden können, ohne erneut authentifizieren verlängern. Die Richtlinie wird angewendet, die Web-API auf diese Weise, wenn die Native App es eine Ressource anfordert, die dieser Richtlinie angewendet werden sollen.

1.  **Erstellen einer Richtlinie Token Lebensdauer.** 

Dieser Befehl wird strenge Richtlinien für eine Web-API erstellen. 
        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"30.00:00:00`",`"MaxAgeMultiFactor`":`"until-revoked`",`"MaxAgeSingleFactor`":`"180.00:00:00`"}}") -DisplayName WebApiDefaultPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy
         
Um Ihre neue Richtlinie und ObjectID erhalten, führen Sie den folgenden Befehl ein.

    Get-AzureADPolicy

&nbsp;&nbsp;2. **die Richtlinie Ihrer Web-API zuweisen**.

Wir werden diese neue Richtlinie mit einer Anwendung zu verknüpfen.  Sie benötigen außerdem eine Möglichkeit, um **ObjectId** Ihrer Anwendung zuzugreifen. Die beste Methode zum Ermitteln Ihrer app **ObjectId** ist der [Azure-Portal](https://portal.azure.com/)zu verwenden. 

Nachdem Sie die **ObjectId**haben, führen Sie den folgenden Befehl ein.

    Add-AzureADApplicationPolicy -ObjectId <ObjectID of the App> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **war's schon!** 

### <a name="sample-managing-an-advanced-policy"></a>Beispiel: Verwalten von Richtlinie Erweitert 

In diesem Beispiel erstellen wir einige Richtlinien zum veranschaulichen Priorität Funktionsweise des, und wie Sie mehrere auf mehrere Objekte angewendete Richtlinien verwalten können. Dies sind einige Einsicht in die Priorität der oben erläuterten Richtlinien und helfen Ihnen, Verwalten von komplexen Szenarien auch. 

1.  **Erstellen einer Richtlinie Token Lebensdauer**

Bisher ziemlich einfach. Wir haben eine Standardrichtlinie Mandanten erstellt haben, die die einfache Aktualisierung Token Lebensdauer auf 30 Tage fest. 

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"30.00:00:00`"}}") -DisplayName ComplexPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
Wie Sie Ihre neue Richtlinie anzeigen und schnell wird ObjectID, führen Sie den folgenden Befehl
 
    Get-AzureADPolicy

&nbsp;&nbsp;2. **weisen Sie die Richtlinie zu einem Dienst Tilgungsanteile**

Jetzt haben wir eine Richtlinie den gesamten Mandanten.  Angenommen, wir dieser Richtlinie 30 Tagen für einen bestimmten Dienst Tilgungsanteile beibehalten, aber die Standardrichtlinie Mandanten benutzerspezifisch obere Grenze von "bis-widerrufen" ändern möchten. 

Zunächst wird gezeigt, diese neue Richtlinie mit unseren Dienst Tilgungsanteile verknüpfen.  Sie benötigen außerdem eine Möglichkeit zum **ObjectId** von Ihrem Dienst Hauptbenutzer zugreifen. Die [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) -Abfragen oder wechseln Sie zu unseren [Graph-Explorer-Tools](https://graphexplorer.cloudapp.net/) , und melden Sie sich bei Ihrem Konto Azure AD-in Ihrem Mandanten Dienst Hauptbenutzer finden Sie unter. 

Nachdem Sie die **ObjectId**haben, führen Sie den folgenden Befehl ein.

    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **Legen Sie die Kennzeichnung IsTenantDefault falsch mit den folgenden Befehl aus**. 

    Set-AzureADPolicy -ObjectId <ObjectId of Policy> -DisplayName ComplexPolicyScenario -IsTenantDefault $false
&nbsp;&nbsp;4. **Erstellen einer neuen Mandanten Standardformat Richtlinie**

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName ComplexPolicyScenarioTwo -IsTenantDefault $true -Type TokenLifetimePolicy

&nbsp;&nbsp;5. **war's schon!** 

Sie verfügen nun über die ursprüngliche Richtlinie in Verbindung mit Ihrem Dienst Tilgungsanteile und die neue Richtlinie als Ihrem Mandanten Standardrichtlinie festlegen.  Es ist wichtig, denken Sie daran, auf Dienst Hauptbenutzer angewendete Richtlinien Priorität über Mandanten Standardrichtlinien angezeigt. 


## <a name="cmdlet-reference"></a>Cmdletreferenz zur

### <a name="manage-policies"></a>Verwalten von Richtlinien
Die folgenden Cmdlets kann zum Verwalten von Richtlinien verwendet werden.</br></br>



#### <a name="new-azureadpolicy"></a>Neue AzureADPolicy
Erstellt eine neue Richtlinie an.

    New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsTenantDefault <boolean> -Type <Policy Type> 

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-Definition| Das Array von als Zeichenfolgen dargestellt JSON, die alle Regeln der Richtlinie enthält.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
DisplayName-|Zeichenfolge des Namens der Richtlinie|DisplayName - MyTokenPolicy
-IsTenantDefault|Wenn wahr die Richtlinie als legt hat Mandanten Standardrichtlinie, wenn der Wert false keine Auswirkung|-IsTenantDefault $true
-Typ|Verwenden Sie der Typ der Richtlinie, für die token Lebensdauer immer "TokenLifetimePolicy"|-TokenLifetimePolicy Datentyp
-AlternativeIdentifier [Optional]|Legt eine alternative Id auf die Richtlinie an.|AlternativeIdentifier - myAltId
</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy         
Ruft alle AzureAD Richtlinien oder angegebene Richtlinie 
        
    Get-AzureADPolicy 

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId [Optional]|Das Objekt-Id die Richtlinie, die Sie erhalten möchten. |ObjectId - &lt;ObjectID für die Richtlinie&gt; 
</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject         
Ruft alle apps und Dienst Hauptbenutzer mit einer Richtlinie verknüpft sind
        
    Get-AzureADPolicyAppliedObject -ObjectId <object id of policy> 

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id die Richtlinie, die Sie erhalten möchten.|ObjectId - &lt;ObjectID für die Richtlinie&gt;
</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Eine vorhandene Richtlinie aktualisiert
        
    Set-AzureADPolicy -ObjectId <object id of policy> -DisplayName <string> 
 
Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id die Richtlinie, die Sie erhalten möchten.|ObjectId - &lt;ObjectID für die Richtlinie&gt;
DisplayName-|Zeichenfolge des Namens der Richtlinie|DisplayName - MyTokenPolicy
-Definition [Optional]|Das Array von als Zeichenfolgen dargestellt JSON, die alle Regeln der Richtlinie enthält.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-IsTenantDefault [Optional]|Wenn wahr die Richtlinie als legt hat Mandanten Standardrichtlinie, wenn der Wert false keine Auswirkung|-IsTenantDefault $true
-Type [Optional]|Verwenden Sie der Typ der Richtlinie, für die token Lebensdauer immer "TokenLifetimePolicy"|-TokenLifetimePolicy Datentyp
-AlternativeIdentifier [Optional]|Legt eine alternative Id auf die Richtlinie an.|AlternativeIdentifier - myAltId
</br></br>

#### <a name="remove-azureadpolicy"></a>Entfernen-AzureADPolicy         
Löscht die angegebene Richtlinie

     Remove-AzureADPolicy -ObjectId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id die Richtlinie, die Sie erhalten möchten.|ObjectId - &lt;ObjectID für die Richtlinie&gt;
</br></br>

### <a name="application-policies"></a>Anwendungsrichtlinien
Für die von Anwendungsrichtlinien kann die folgenden Cmdlets verwendet werden.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Hinzufügen von AzureADApplicationPolicy         
Die angegebene Richtlinie Links zur Anwendung

    Add-AzureADApplicationPolicy -ObjectId <object id of application> -RefObjectId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID der Anwendung&gt; 
-RefObjectId|Das Objekt-Id die Richtlinie. |RefObjectId - &lt;ObjectID für die Richtlinie&gt;
</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy        
Die Richtlinie zur Anwendung zugewiesen wird

    Get-AzureADApplicationPolicy -ObjectId <object id of application>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID der Anwendung&gt; 
</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Entfernen-AzureADApplicationPolicy        
Entfernt eine Richtlinie aus einer Anwendung

    Remove-AzureADApplicationPolicy -ObjectId <object id of application> -PolicyId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID der Anwendung&gt; 
-PolicyId| Die ObjectId für die Richtlinie.|PolicyId - &lt;ObjectID für die Richtlinie&gt;
</br></br>

### <a name="service-principal-policies"></a>Richtlinien für Hauptbenutzer
Die folgenden Cmdlets kann für Hauptbenutzer Dienstrichtlinien verwendet werden.</br></br>

#### <a name="add-azureadserviceprincipalpolicy"></a>Hinzufügen von AzureADServicePrincipalPolicy         
Die angegebene Richtlinie Links zu einem Dienst Tilgungsanteile

    Add-AzureADServicePrincipalPolicy -ObjectId <object id of service principal> -RefObjectId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID der Anwendung&gt; 
-RefObjectId|Das Objekt-Id die Richtlinie. |RefObjectId - &lt;ObjectID für die Richtlinie&gt;
</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy        
Ruft alle Richtlinien, die mit der angegebenen Dienst Tilgungsanteile verknüpft sind

    Get-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>
 
Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID der Anwendung&gt; 
</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Entfernen-AzureADServicePrincipalPolicy         
Löscht die Richtlinie aus einem angegebenen Dienst Hauptbenutzer

    Remove-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>  -PolicyId <object id of policy>
 
Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Das Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID der Anwendung&gt; 
-PolicyId| Die ObjectId für die Richtlinie.|PolicyId - &lt;ObjectID für die Richtlinie&gt;
