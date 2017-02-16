<properties
    pageTitle="Azure Active Directory Federation Metadaten | Microsoft Azure"
    description="In diesem Artikel werden das Föderation Metadatendokument, das Azure Active Directory für Dienste veröffentlicht, die Azure Active Directory Token akzeptieren."
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


# <a name="federation-metadata"></a>Föderation Metadaten

Azure Active Directory (Azure AD) eine Föderation-Metadaten-Dokument für Dienste, die so konfiguriert, dass die Sicherheitstoken akzeptieren, die Probleme Azure AD veröffentlicht. Das Format des Dokuments Föderation Metadaten wird in der [Web Services Federation Language (WS-Verbund), Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), beschrieben, die [Metadaten für die OASIS Security Assertion Markup Language (SAML) 2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf)erweitert.

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Mandanten-spezifische und Mandanten unabhängig Metadatenendpunkte

Azure AD veröffentlicht Mandanten-spezifische und Mandanten unabhängig Endpunkte.

Mandanten-spezifische Endpunkte dienen für einen bestimmten Mandanten. Mandanten-spezifische Föderation Metadaten enthält Informationen zu den Mandanten, einschließlich der Mandanten-spezifische Informationen zu Herausgeber und Endpunkt. Anwendungen, die Einschränken des Zugriffs auf einen einzelnen Mandanten verwenden Mandanten-spezifische Endpunkte.

Mandanten unabhängig Endpunkte finden Sie Informationen, die alle Azure AD-Mandanten gemeinsam ist. Diese Informationen gilt für Mandanten am *login.microsoftonline.com* gehostet und gesamten Mandanten gemeinsam genutzten. Mandanten unabhängig Endpunkte werden für Applikationen mit mehreren Mandanten, empfohlen, da sie nicht mit einem beliebigen bestimmten Mandanten zugeordnet sind.

## <a name="federation-metadata-endpoints"></a>Föderation Metadatenendpunkte

Azure AD veröffentlicht Föderation Metadaten bei `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Für den **Mandanten-spezifische Endpunkte**die `TenantDomainName` kann eine der folgenden Arten:

- Registrierte Domänennamen von einer Azure AD-Mandanten, z. B.: `contoso.onmicrosoft.com`.
- Die unveränderlich Mandanten ID der Domäne, z. B. `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Für den **Mandanten unabhängig Endpunkte**die `TenantDomainName` ist `common`. Dieses Dokument enthält nur die Elemente Föderation Metadaten, die alle Azure AD-Mandanten feldbezogene, die am login.microsoftonline.com gehostet werden.

Zum Beispiel möglicherweise ein Endpunkt Mandanten-spezifische `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Der Endpunkt Mandanten unabhängig ist [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Sie können Föderation Metadatendokument anzeigen, indem Sie diese URL in einem Browser eingeben.

## <a name="contents-of-federation-metadata"></a>Inhalt der Föderation Metadaten

Der folgende Abschnitt enthält Informationen, die Dienste, die die Token ausgestellt von Azure AD nutzen benötigt.

### <a name="entity-id"></a>Element-ID

Die `EntityDescriptor` Element enthält ein `EntityID` Attribut. Der Wert der `EntityID` Attribut stellt den Herausgeber, d. h., den Security token Service (STS), die das Token ausgestellt. Es ist wichtig, den Herausgeber zu überprüfen, wenn Sie ein Token erhalten.

Die folgende Metadaten zeigt ein Beispiel des Mandanten-spezifische `EntityDescriptor` Element mit einer `EntityID` Element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Sie können die Mandanten-ID in den Endpunkt des Mandanten unabhängig ersetzen, mit Ihrer Mandanten-ID ein für einen bestimmten Mandanten erstellen `EntityID` Wert. Der Ergebniswert wird der Tokenausgeber identisch sein. Die Strategie ermöglicht eine Anwendung mit mehreren Mandanten zum Überprüfen der Herausgeber für einen angegebenen Mandanten.

Die folgende Metadaten zeigt ein Beispiel des Mandanten unabhängig `EntityID` Element. Bitte beachten Sie, dass die `{tenant}` ist ein Literal, die nicht in einem Platzhalter.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Tokensignaturzertifikate

Wenn ein Dienst ein Token, die von einem Azure AD-Mandanten ausgegeben wird erhält, muss die Signatur das Token mit einem signierenden Schlüssel überprüft werden, die im Dokument Metadaten Föderation veröffentlicht wird. Die Föderation-Metadaten enthalten den öffentlichen Teil der Zertifikate, mit denen die Mandanten zum token signieren. Die unformatierten Bytes des Zertifikats angezeigt, der `KeyDescriptor` Element. Das Signaturzertifikat Token ist gültig für die Signatur nur, wenn der Wert der `use` Attribut ist `signing`.

Eine Föderation Metadatendokument veröffentlicht von Azure AD kann mehrere signierende Tasten, z. B. wenn Azure AD Vorbereiten des Signaturzertifikats aktualisieren müssen. Wenn eine Föderation Metadatendokument mehr als ein Zertifikat enthält, sollte ein Dienst, der die Gültigkeit die Token überprüft alle Zertifikate im Dokument unterstützen.

Die folgende Metadaten zeigt ein Beispiel `KeyDescriptor` Element mit einem signierenden Schlüssel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Die `KeyDescriptor` Element wird an zwei Stellen im Dokument Metadaten Föderation. im Abschnitt SAML-spezifische und WS-Verbund-spezifischen Abschnitt. Die Zertifikate in beiden Abschnitten veröffentlicht werden identisch sein.

Im Abschnitt WS-Verbund-spezifische würde ein WS-Verbund-Metadaten-Leser lesen Sie die Zertifikate aus einer `RoleDescriptor` Element mit der `SecurityTokenServiceType` Typ.

Die folgende Metadaten zeigt ein Beispiel `RoleDescriptor` Element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

Im Abschnitt SAML-spezifische würde ein WS-Verbund-Metadaten-Leser lesen Sie die Zertifikate aus einer `IDPSSODescriptor` Element.

Die folgende Metadaten zeigt ein Beispiel `IDPSSODescriptor` Element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Es gibt keine Unterschiede in das Format des Mandanten-spezifische und Mandanten unabhängig Zertifikate.

### <a name="ws-federation-endpoint-url"></a>WS-Verbund Endpunkt-URL

Die Föderation Metadaten enthält die URL, Azure AD-verwendet für-in und Single Abmeldung in WS-Verbund Protokoll ist. Dieser Endpunkt wird in der `PassiveRequestorEndpoint` Element.

Die folgende Metadaten zeigt ein Beispiel `PassiveRequestorEndpoint` Element für einen Mandanten-spezifische Endpunkt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Für den Mandanten unabhängig-Endpunkt wird die WS-Verbund-URL in den Endpunkt WS-Verbund angezeigt, wie im folgenden Beispiel gezeigt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML-Protokoll Endpunkt-URL

Die Föderation Metadaten enthält die URL, die Azure AD-Ins und Single Abmeldung in SAML 2.0-Protokoll verwendet. Diese Grenzwerte werden in der `IDPSSODescriptor` Element.

Die URLs anmelden und Abmelden angezeigt wird, der `SingleSignOnService` und `SingleLogoutService` Elemente.

Die folgende Metadaten zeigt ein Beispiel `PassiveResistorEndpoint` für einen Mandanten-spezifische Endpunkt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Auf ähnliche Weise werden die Endpunkte für die gemeinsame SAML 2.0-Protokoll Endpunkte in den Metadaten Mandanten unabhängig Föderation veröffentlicht, wie im folgenden Beispiel dargestellt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
