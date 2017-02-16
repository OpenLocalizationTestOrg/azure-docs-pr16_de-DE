<properties 
    pageTitle="Service Bus Authentifizierung und Autorisierung | Microsoft Azure"
    description="Übersicht über freigegebene Access Signatur (SAS) Authentifizierung."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Dienstbus Authentifizierung und Autorisierung

Applikationen können Azure Service unter Verwendung mit entweder Authentifizierung freigegeben Access Signatur (SAS) oder über Azure Active Directory Access-Steuerelement (auch bekannt als Steuerelement-Dienst oder ACS) authentifizieren. Freigegebene Access Signatur Authentifizierung ermöglicht Applikationen unter Verwendung einer Zugriffstaste konfiguriert den Namespace, oder klicken Sie auf die Entität, auf der bestimmte Rechte zugeordnet sind mit Dienst authentifizieren. Dieser Taste können dann um ein Token freigegeben Access Signatur zu erzeugen, die Clients Dienstbus authentifizieren verwendet werden kann.

>AZURE. WICHTIGE SAS empfiehlt sich über ACS, wie sie eine einfache, flexible und einfach zu verwendendes Authentifizierung des Farbschemas für Dienstbus bereitstellt. Anwendung kann SAS in Szenarien verwenden, in denen sie nicht benötigen zum Verwalten des Begriff "Benutzerberechtigung."

## <a name="shared-access-signature-authentication"></a>Freigegebene Access Signatur-Authentifizierung

[SAS-Authentifizierung](service-bus-sas-overview.md) können Sie einem Benutzerzugriff zu Dienstbus Ressourcen mit bestimmten Rechte gewähren. SAS-Authentifizierung in Dienstbus umfasst die Konfiguration eines cryptographic Schlüssels mit zugeordneten Berechtigungen für eine Ressource Dienstbus. Clients können danach Zugriff auf diese Ressource durch Vorführung ein SAS Token besteht aus den Ressourcen-URI zugegriffen wird und ein Ablauf mit dem konfigurierten Schlüssel signiert.

Sie können auf einen Namespace Dienstbus Schlüssel für SAS konfigurieren. Der Schlüssel für alle per Personen in diesem Namespace angewendet wird. Sie können auch die Tasten Servicebuswarteschlangen und Themen konfigurieren. SAS wird ebenfalls unterstützt, klicken Sie auf Dienstbus weiterleitet.

Wenn SAS verwenden möchten, können Sie ein Objekt [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) konfigurieren, auf einen Namespace, Warteschlange oder Thema, das die folgenden Aktionen aus:

- *Schlüssel* , die die Regel identifiziert.

- *PrimaryKey* ist eine cryptographic-Taste, melden Sie sich/SAS Token zu bestätigen.

- *Sekundärer Schlüssel* ist eine cryptographic-Taste, melden Sie sich/SAS Token zu bestätigen.

- *Rechte* , die die Sammlung von Listen, senden oder verwalten Rechte gewährt darstellt.

Autorisierungsregeln Ebene des Namespace konfiguriert können Clients Zugriff auf alle Elemente in einem Namespace mit mit dem entsprechenden Schlüssel signierten Token gewähren. Bis zu 12 solche Autorisierung können Regeln auf einem Dienstbus Namespace, Warteschlange oder das Thema konfiguriert sein. Standardmäßig ist ein [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) alle Rechte für jeden Namespace konfiguriert Wenn es zuerst bereitgestellt wird.

Um eine Entität zugreifen zu können, muss der Client ein SAS Token mit einer bestimmten [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)generiert. Das SAS Token wird generiert mithilfe der HMAC-SHA256 einer Ressourcenzeichenfolge, die der der Ressource-URI besteht, der Zugriff in Anspruch genommen wird, und eine Ablauf einen cryptographic Key die Autorisierungsregel zugeordnet.

SAS-Authentifizierung Unterstützung für Dienstbus ist in Azure .NET SDK, Version 2.0 und höher enthalten. SAS unterstützt ein [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Unterstützung für SAS Verbindungszeichenfolgen einbeziehen alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren

## <a name="acs-authentication"></a>ACS-Authentifizierung

Dienstbus-Authentifizierung über ACS über eine Ergänzung verwaltet wird "-Sb" ACS-Namespace. Wenn Sie einen Companion ACS-Namespace für einen Namespace Dienstbus erstellt werden möchten, können keine Ihren Dienstbus Namespace mit dem klassischen Azure-Portal erstellt werden; Sie müssen den Namespace des PowerShell-Cmdlets [New-AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) mit erstellen. Beispiel:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Um zu vermeiden, erstellen einen Namespace ACS, geben Sie den folgenden Befehl aus:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Wenn Sie einen Dienstbus Namespace mit dem Namen **contoso.servicebus.windows.net**erstellen, wird beispielsweise automatisch ein Companion ACS-Namespace namens **Contoso-sb.accesscontrol.windows.net** bereitgestellt. Für alle Namespaces, die vor August 2014 erstellt wurden, wurde ein zugehörigen ACS-Namespace auch erstellt.

Eine Standard-Dienstidentität "Besitzer" alle Rechte aufweist, wird standardmäßig in diesem Companion ACS-Namespace bereitgestellt. Konfigurieren der entsprechenden Trust "Beziehungen" können Sie abgestimmte Steuerelement auf jede Dienstbus Entität über ACS abrufen. Sie können zusätzlichen Dienstidentitäten für das Verwalten des Zugriffs für Personen Dienstbus konfigurieren.

Um eine Entität zugreifen, fordert der Client ein SWT Token aus ACS mit den entsprechenden Ansprüchen durch Vorführung seine Anmeldeinformationen. Das Token SWT muss dann als Teil der Anfrage an Dienstbus gesendet die Autorisierung des Clients für den Zugriff auf die Entität zu aktivieren.

ACS-Authentifizierung Unterstützung für Dienstbus ist in Azure .NET SDK, Version 2.0 und höher enthalten. Diese Authentifizierung unterstützt ein [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). Unterstützung für ACS-Verbindungszeichenfolgen einbeziehen alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren

## <a name="next-steps"></a>Nächste Schritte

Weiter zu [Access-Signatur freigegebene Authentifizierung mit Dienstbus](service-bus-shared-access-signature-authentication.md) ausführliche Informationen zu SAS lesen.

Eine Übersicht über SA-in Dienstbus finden Sie unter [Freigegebene Access Signaturen](service-bus-sas-overview.md).

Sie können finden Sie weitere Informationen zu ACS-Token in [wie: ACS über das OAuth UMBRECHEN Protokoll ein Token beantragen](https://msdn.microsoft.com/library/hh674475.aspx).



