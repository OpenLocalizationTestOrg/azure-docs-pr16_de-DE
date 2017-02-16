<properties
    pageTitle="Azure einzelnen Abmelden SAML-Protokoll | Microsoft Azure"
    description="In diesem Artikel werden die einzelnen Sign-Out SAML-Protokoll in Azure Active Directory"
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


# <a name="single-sign-out-saml-protocol"></a>Einzelne Abmeldung SAML-Protokoll

Azure Active Directory (Azure AD) unterstützt das SAML 2.0 web Browser einzelnes Abmeldung Profil. Für einzelne Abmeldung muss um ordnungsgemäß funktionieren Azure AD-URL den Metadaten während der Registrierung der Anwendung registrieren. Azure AD Ruft die URL für die Abmeldung und den signierenden Schlüssel Cloud-Dienst aus den Metadaten ab. Azure AD verwendet den signierenden Schlüssel zum Überprüfen der Signatur der eingehenden LogoutRequest, und es wird die LogoutURL verwendet, um Benutzer umzuleiten, nachdem sie sich angemeldet sind.

Wenn der Cloud-Dienst einen Metadaten-Endpunkt nicht unterstützt, nachdem die Anwendung registriert ist, muss der Entwickler Microsoft Support, um die URL abmelden und signierenden Schlüssel bereitstellen wenden.

Dieses Diagramm zeigt den Workflow für die einzelnen Abmeldung Azure AD-Prozess.

![Einzelne Abmelden Workflow](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Sendet der Cloud-Dienst eine `LogoutRequest` Nachricht Azure AD um anzugeben, dass eine Sitzung beendet wurde. Der folgende Codeausschnitt zeigt ein Beispiel `LogoutRequest` Element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

Die `LogoutRequest` Azure AD gesendet Element erfordert die folgenden Attribute:

- `ID`: Dies kennzeichnet die Abmelde Anforderung aus. Der Wert der `ID` sollten nicht mit einer Zahl beginnen. Die übliche Vorgehensweise besteht darin **Id** an die Zeichenfolge Darstellung einer GUID angefügt werden soll.

- `Version`: Legen Sie den Wert für dieses Element auf **2.0**ein. Dieser Wert ist erforderlich.

- `IssueInstant`: Dies ist eine `DateTime` Zeichenfolge mit einem Wert koordinieren Coordinated Universal Time (UTC) und [Roundtripformat ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD erwartet einen Wert dieses Typs, aber es werden keine erzwungen.

- Die `Consent`, `Destination`, `NotOnOrAfter` und `Reason` Attribute werden ignoriert, wenn sie in enthalten sind eine `LogoutRequest` Element.

### <a name="issuer"></a>Herausgeber

Die `Issuer` Element in einer `LogoutRequest` muss einer der in der Cloud-Dienst die **ServicePrincipalNames** genau in Azure AD entsprechen. Dies wird in der Regel auf der **App-ID-URI** festgelegt, die während der Anwendung Registrierung angegeben ist.

### <a name="nameid"></a>NameID

Den Wert von der `NameID` Element muss exakt die `NameID` des Benutzers, die sich signiert werden.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD-sendet eine `LogoutResponse` als Antwort auf eine `LogoutRequest` Element. Der folgende Codeausschnitt zeigt ein Beispiel `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD-Datensätze der `ID`, `Version` und `IssueInstant` Werte in der `LogoutResponse` Element. Wird die `InResponseTo` Element auf den Wert von der `ID` Attribut von der `LogoutRequest` , die die Antwort Antikörpermusters.

### <a name="issuer"></a>Herausgeber

Azure AD verwenden: legt diesen Wert auf `https://login.microsoftonline.com/<TenantIdGUID>/` , in dem <TenantIdGUID> ist die Mandanten-ID des Azure AD-Mandanten.

Für den Wert, der ausgewertet werden soll die `Issuer` Element, verwenden Sie der Wert des **App-ID-URI** während der Registrierung der Anwendung bereitgestellt.

### <a name="status"></a>Status

Azure AD verwendet die `StatusCode` Element in der `Status` Element zum Erfolg oder Fehler bei der Abmeldung anzuzeigen. Wenn die Abmelde schlägt fehl, die `StatusCode` Element kann auch benutzerdefinierte Fehlermeldungen enthalten.
