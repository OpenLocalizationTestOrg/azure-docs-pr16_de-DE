<properties
    pageTitle="Azure für einmaliges Anmelden SAML-Protokoll | Microsoft Azure"
    description="In diesem Artikel werden das einzelnen melden Sie sich auf SAML-Protokoll in Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="single-sign-on-saml-protocol"></a>Einzelnes anmelden SAML-Protokoll

Dieser Artikel behandelt das SAML 2.0 Authentifizierung Besprechungsanfragen und Antworten, die unterstützt Azure Active Directory (Azure AD) für einmaliges Anmelden.

Im nachstehenden Diagramm für Protokoll beschreibt die einzelne Sequenz anmelden. Cloud-Dienst (der Dienstanbieter) verwendet eine HTTP-Umleitung Bindung übergeben einer `AuthnRequest` (Authentifizierung) Anforderungselement Azure AD-(der Identitätsanbieter). Dann Azure AD verwendet HTTP Post Binden an Posten eines `Response` Element in der Cloud-Service.

![Einmaliges Anmelden Workflow](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Um eine Benutzerauthentifizierung anzufordern, Cloud services Senden einer `AuthnRequest` Element Azure AD. Einer Stichprobe SAML 2.0 `AuthnRequest` kann wie folgt aussehen:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| ID | Erforderlich | Azure AD verwendet dieses Attribut zum Füllen der `InResponseTo` Attribut der zurückgegebene Antwort. ID dürfen nicht mit einer Zahl beginnen, damit eine gemeinsame Strategie besteht darin, eine Zeichenfolge wie "Id" in der Zeichenfolge Darstellung einer GUID vorangestellt. Beispielsweise `id6c1c178c166d486687be4aaf5e482730` ist eine gültige ID. |
| Version | Erforderlich | Dies sollte **2.0**sein.|
| IssueInstant | Erforderlich | Dies ist eine DateTime-Zeichenfolge mit einem UTC-Wert und [Roundtripformat ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD erwartet einen DateTime-Wert dieses Typs, jedoch keine bewerten oder verwenden Sie den Wert. |
| AssertionConsumerServiceUrl | Optional | Zur Verfügung gestellt, entsprechen dies die `RedirectUri` des Cloud-Dienst in Azure AD-. |
| ForceAuthn | Optional | Wenn bereitgestellt, sollte false sein. Ein anderer Wert führt zu einem Fehler.|
| IsPassive | Optional | Wenn bereitgestellt, sollte false sein. Ein anderer Wert führt zu einem Fehler. |  

Alle anderen `AuthnRequest` Attribute, z. B. Zustimmung, Ziel, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex und ProviderName werden **ignoriert**.

Azure AD ignoriert, auch die `Conditions` Element in `AuthnRequest`.

### <a name="issuer"></a>Herausgeber

Die `Issuer` Element in einer `AuthnRequest` muss einer der in der Cloud-Dienst die **ServicePrincipalNames** genau in Azure AD entsprechen. Dies wird in der Regel auf der **App-ID-URI** festgelegt, die während der Anwendung Registrierung angegeben ist.

Eine Stichprobe SAML Ausschnitt mit der `Issuer` Element sieht wie folgt aus:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Dieses Element Format für einen bestimmten Name-ID in der Antwort anfordert, und ist optional in `AuthnRequest` Elemente an Azure AD gesendet werden.

Einer Stichprobe `NameIdPolicy` Element sieht wie folgt aus:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Wenn `NameIDPolicy` wird, sofern Sie deren optional aufnehmen können `Format` Attribut. Die `Format` Attribut kann nur einen der folgenden Werte; aufweisen ein anderer Wert führt zu einem Fehler.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure-Active Directory Probleme den NameID Anspruch als paarweiser Bezeichner.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure-Active Directory Probleme den NameID Anspruch im Format von e-Mail-Adresse.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Dieser Wert explizit gestattet Azure Active Directory das anfordern Format auswählen. Azure-Active Directory Probleme der NameID als paarweiser Bezeichner.

Nehmen Sie nicht die `SPNameQualifer` Attribut. Azure AD ignoriert die `AllowCreate` Attribut.

### <a name="requestauthncontext"></a>RequestAuthnContext

Die `RequestedAuthnContext` Element gibt die gewünschten Authentifizierungsmethoden. Es ist optional in `AuthnRequest` Elemente an Azure AD gesendet werden. Azure AD unterstützt nur eine `AuthnContextClassRef` Wert: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Definieren von Bereichen

Die `Scoping` Element, das eine Liste der Identitätsanbieter enthält, ist optional in `AuthnRequest` Elemente an Azure AD gesendet werden.

Wenn bereitgestellt hat, nehmen Sie nicht die `ProxyCount` Attribut, `IDPListOption` oder `RequesterID` Element, wie sie nicht unterstützt werden.

### <a name="signature"></a>Signatur

Nehmen Sie keiner `Signature` Element in `AuthnRequest` Elemente, wie Azure AD nicht unterstützt Authentifizierungsanfragen angemeldet.

### <a name="subject"></a>Betreff

Azure AD ignoriert die `Subject` Element des `AuthnRequest` Elemente.

## <a name="response"></a>Antwort

Wenn eine angeforderte Anmeldung erfolgreich abgeschlossen wurde, Beiträge Azure AD eine Antwort auf die Cloud-Dienst an. Eine Beispielantwort auf einen erfolgreichen Versuch anmelden sieht wie folgt aus:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Antwort

Die `Response` Element enthält das Ergebnis der Anfrage Autorisierung. Azure AD-Datensätze der `ID`, `Version` und `IssueInstant` Werte in der `Response` Element. Er legt auch die folgenden Attributen:

- `Destination`: Wenn anmelden erfolgreich abgeschlossen ist, ist dies festgelegt, dass die `RedirectUri` des Dienstanbieters (Cloud-Dienst).
- `InResponseTo`: Dies ist zum Festlegen der `ID` Attribut von der `AuthnRequest` Element, das die Antwort initiiert.

### <a name="issuer"></a>Herausgeber

Azure AD-Datensätze der `Issuer` Element `https://login.microsoftonline.com/<TenantIDGUID>/` , in dem <TenantIDGUID> ist die Mandanten-ID des Azure AD-Mandanten.

Eine Stichprobe Antwort mit Herausgeber Element könnte beispielsweise wie folgt aussehen:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Status

Die `Status` Element übermittelte Erfolg oder Fehler bei der Anmeldung. Er enthält die `StatusCode` Element, das enthält einen Code oder eine Reihe von verschachtelten Funktionen, die den Status der Anfrage darstellen. Darüber hinaus enthält die `StatusMessage` Element, das benutzerdefinierte Fehlermeldungen enthält, die während der Anmeldung generiert werden.

<!-- TODO: Add a authentication protocol error reference -->

Im folgenden finden eine SAML-Antwort auf einen nicht erfolgreich anmelden Versuch.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Assertion

Zusätzlich zu den `ID`, `IssueInstant` und `Version`, Azure AD legt die folgenden Elemente der `Assertion` Element der Antwort.

#### <a name="issuer"></a>Herausgeber

Dies ist auf festgelegt `https://sts.windows.net/<TenantIDGUID>/`, in dem <TenantIDGUID> ist die Mandanten-ID des Azure AD-Mandanten.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Signatur

Azure AD meldet die Assertion als Antwort auf eine erfolgreiche Anmeldung. Die `Signature` Element enthält eine digitale Signatur, die die Quelle, um zu überprüfen, ob die Integrität des die Assertion authentifizieren Cloud-Dienst verwenden können.

Um diese digitale Signatur zu generieren, verwendet Azure AD den signierenden Schlüssel in der `IDPSSODescriptor` Element seine Metadaten Dokuments.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Betreff

Hiermit wird die Tilgungsanteile, die den gewünschten Betreff für die Anweisungen in die Assertion ist. Sie enthält eine `NameID` Element, das den authentifizierten Benutzer darstellt. Die `NameID` Wert ist eine gezielte Bezeichner enthält, die nur für den Dienstanbieter verwiesen wird, das die Zielgruppe, für das Token ist. Es ist beständiger – sie können aufgehoben werden sollen, aber nie neu zugewiesen. Es ist außerdem undurchsichtig, darin, dass es nicht etwas über den Benutzer erkennbar und nicht als Bezeichner für Attribut Abfragen verwendet werden.

Die `Method` Attribut für die `SubjectConfirmation` Element wird immer festgelegt `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Bedingungen

Dieses Element gibt, die die zulässige Verwendung von SAML-Assertionen definieren.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Die `NotBefore` und `NotOnOrAfter` Attribute angeben, das Intervall, bei dem die Assertion gültig ist.

- Der Wert von der `NotBefore` Attribut ist gleich zu oder etwas (weniger als eine Sekunde) später als der Wert der `IssueInstant` Attribut von der `Assertion` Element. Azure AD wird für alle zeitlichen Unterschied zwischen sich selbst und Cloud-Dienst (Service Provider) nicht berücksichtigt, und diesmal keine Puffer hinzu.
- Der Wert von der `NotOnOrAfter` Attribut ist 70 Minuten später als der Wert von der `NotBefore` Attribut.

#### <a name="audience"></a>Zielgruppe

Diese Datei enthält einen URI, eine Zielgruppe identifiziert. Azure AD legt den Wert für dieses Element auf den Wert der `Issuer` Bestandteil der `AuthnRequest` , die die Anmeldung initiiert. Auswerten der `Audience` Wert, verwenden Sie den Wert von der `App ID URI` , der während der Anwendung Registrierung angegeben wurde.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Wie die `Issuer` Wert der `Audience` Wert muss exakt einen der wichtigsten Namen, die Cloud-Dienst in Azure AD darstellt. Jedoch wenn der Wert von der `Issuer` Element ist kein URI-Wert, der `Audience` Wert in der Antwort ist die `Issuer` Wert mit dem Präfix `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Dies enthält Ansprüche zu den Betreff oder den Benutzer aus. Im folgende Ausschnitt enthält ein Beispiel `AttributeStatement` Element. Die drei Punkte gibt an, dass das Element mehrere Attribute und Attributwerte enthalten sein kann.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Namen anfordern** : den Wert von der `Name` Attribut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) ist der wichtigsten Benutzername der authentifizierten Benutzer wie `testuser@managedtenant.com`.
- **ObjectIdentifier anfordern** : den Wert von der `ObjectIdentifier` Attribut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) ist die `ObjectId` des Directory-Objekts, das den authentifizierten Benutzer in Azure AD darstellt. `ObjectId`ist unveränderlich, global eindeutig und sicheren Bezeichner für den authentifizierten Benutzer wieder verwenden.

#### <a name="authnstatement"></a>AuthnStatement

Dieses Element bestimmt, dass auf der Betreff Assertion mit einer bestimmten Mitteln zu einem bestimmten Zeitpunkt authentifiziert wurde.

- Die `AuthnInstant` Attribut gibt die Zeit an der Benutzerauthentifizierung mit Azure AD.
- Die `AuthnContext` Element gibt Authentifizierung Kontext zum Authentifizieren des Benutzers verwendet.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
