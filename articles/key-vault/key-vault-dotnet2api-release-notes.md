<properties
   pageTitle="Wichtiger Tresor .NET 2.x-API Versionsinformationen | Microsoft Azure"
   description=".NET Entwickler werden diese API Code für Azure-Taste Tresor verwendet."
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Tresor .NET 2.0 - Versionsinformationen und Migrationshandbuch

Die folgenden Hinweise und Notizen sind für das Arbeiten mit der Azure-Taste Tresor .NET Entwickler / C#-Bibliothek. In die Änderung der Version 2.0 aus der Version 1.0 eine Reihe von Updates wurden die Migration Arbeit im Code in Reihenfolge Sie den funktionsübergreifendes Verbesserungen nutzbringend und Ergänzungen, wie z. B. Schlüssel Tresor Zertifikate Unterstützung feature erforderlich wird.

Taste Tresor Zertifikate Unterstützung für die Verwaltung von Ihrem X509 bietet Zertifikate und das folgenden Verhalten:  

-   Ermöglicht die Besitzer einer Zertifikat zum Erstellen eines Zertifikats durch eine Taste Tresor Erstellungsvorgang oder durch den Import von ein vorhandenes Zertifikat. Dies umfasst beide selbst signiert und Zertifizierungsstelle generiert Zertifikate.

- Ermöglicht einen Taste Tresor Zertifikat Besitzer sicheren Speicher und die Verwaltung von X509 implementieren Zertifikate ohne Interaktion mit privaten Schlüsseln.  

-   Ermöglicht einen Zertifikat Besitzer eine Richtlinie zu erstellen, die die Taste Tresor zum Verwalten des Lebenszyklus eines Zertifikats weiterleitet.  

-   Ermöglicht das Zertifikat Besitzer für die Benachrichtigung über Ereignisse des Lebenszyklus von Ablauf und Erneuerung des Zertifikats Kontaktinformationen.  

-   Unterstützt die automatische Erneuerung mit ausgewählten Emittenten - Taste Tresor Partner X509 Zertifikat Anbieter / Zertifizierungsstellen.
    - Hinweis: Anbieter/Rechtsgrundlagenverzeichnis-Produkten kombinieren sind ebenfalls zulässig, aber das automatische Erneuerung Feature wird nicht unterstützt.


## <a name="net-support"></a>Unterstützung für .NET
- **.NET 4.0** wird nicht unterstützt, durch die .NET Azure-Taste Tresor, Version 2.0 / C#-Bibliothek
- **.NET Core** wird durch die .NET Azure-Taste Tresor, Version 2.0 unterstützt / C#-Bibliothek

## <a name="namespaces"></a>Namespaces
- Der Namespace für **Modelle** wird von **Microsoft.Azure.KeyVault** zu **Microsoft.Azure.KeyVault.Models**geändert.
- Der Namespace **Microsoft.Azure.KeyVault.Internal** wird gelöscht.
- Namespace Abhängigkeiten Azure SDK aus **Hyak.Common** und **Hyak.Common.Internals** **Microsoft.Rest** und **Microsoft.Rest.Serialization** geändert werden


## <a name="type-changes"></a>Typ ändern
- *Geheim* in *SecretBundle* geändert
- *Wörterbuch* in *IDictionary* geändert
- *Liste<T>, string []* in geändert *IList<T> *
- *NextList* in *NextPageLink* geändert


## <a name="return-types"></a>Typen zurückgeben
- **KeyList** und **SecretList** zurückgegeben werden kann *IPage<T> * anstelle von *ListKeysResponseMessage*
- Die generierten **BackupKeyAsync** zurückgegeben werden *BackupKeyResult* kann die *Wert* (Sicherung Blob) enthält. Bevor die Methode umbrochen wurde und nur der Wert zurückgegeben.

## <a name="exceptions"></a>Ausnahmen
- *KeyVaultClientException* wird in *KeyVaultErrorException* geändert.
- Die Dienstfehler wird von *Ausnahme geändert. Fehler* auf *Ausnahme. Body.Error.Message*.
- Weitere Informationen entfernt über die Fehlermeldung für **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktoren
- Anstelle einer *HttpClient* als Konstruktorargument akzeptiert der Konstruktor nur *HttpClientHandler* oder *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Heruntergeladenen Pakete  
Wenn ein Client eine Abhängigkeit von Key Tresor verarbeitet wurden die folgenden heruntergeladen.
#### <a name="previous-package-list"></a>Liste der vorherigen Paket
- Verpacken id="Hyak.Common" Version = "1.0.2" TargetFramework = "net45"
- Verpacken id="Microsoft.Azure.Common" Version = "2.0.4" TargetFramework = "net45"
- Verpacken id="Microsoft.Azure.Common.Dependencies" Version = "1.0.0" TargetFramework = "net45"
- Verpacken id="Microsoft.Azure.KeyVault" Version = "1.0.0" TargetFramework = "net45"
- Verpacken id="Microsoft.Bcl" Version = "1.1.9" TargetFramework = "net45"
- Verpacken id="Microsoft.Bcl.Async" Version = "1.0.168" TargetFramework = "net45"
- Verpacken id="Microsoft.Bcl.Build" Version = "1.0.14" TargetFramework = "net45"
- Verpacken id="Microsoft.Net.Http" Version = "2.2.22" TargetFramework = "net45"

#### <a name="current-package-list"></a>Aktuelle Paketliste
- Verpacken id="Microsoft.Azure.KeyVault" Version = "2.0.0-preview" TargetFramework = "net45"
- Verpacken id="Microsoft.Rest.ClientRuntime" Version = "2.2.0" TargetFramework = "net45"
- Verpacken id="Microsoft.Rest.ClientRuntime.Azure" Version = "3.2.0" TargetFramework = "net45"


## <a name="class-changes"></a>Verzicht Änderungen

- **UnixEpoch** Klasse wurde entfernt
- **Base64UrlConverter** -Klasse ist in **Base64UrlJsonConverter** umbenannt.

## <a name="other-changes"></a>Andere Änderungen

- Unterstützung für die Konfiguration der KV Vorgang "Wiederholen" Richtlinie auf vorübergehende Fehler wurde in dieser Version der API hinzugefügt.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Für die Vorgänge, die ein *Tresor*zurückgegeben wird, wurde der Rückgabetyp einer Klasse, die eine Eigenschaft Tresor enthalten. Der Rückgabetyp ist jetzt *Tresor*.
- *PermissionsToKeys* und *PermissionsToSecrets* sind jetzt *Permissions.Keys* und *Permissions.Secrets*
- Einige der Rückgabetyp Typen Änderungen gelten für das Steuerelement-Ebene sowie.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Das Paket ist für die Verschlüsselung Vorgänge **Microsoft.Azure.KeyVault.Extensions** und **Microsoft.Azure.KeyVault.Cryptography** aufgeteilt.
