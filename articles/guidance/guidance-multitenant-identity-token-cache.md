<properties
   pageTitle="Access-Token in einer Anwendung mandantenfähigen Zwischenspeichern | Microsoft Azure"
   description="Zwischenspeichern von Access Token zum Aufrufen der Back-End-Web-API verwendet"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="mwasson"/>


# <a name="caching-access-tokens-in-a-multitenant-application"></a>Access-Token in einer Anwendung mandantenfähigen Zwischenspeichern

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

Es ist eine OAuth Access token, erhältlich, da es sich um eine HTTP-Anforderung an den Endpunkt token erfordert relativ teure. Daher ist es sinnvoll, zu Cache Token, wann immer möglich. [Azure AD-Authentifizierungsbibliothek] [ ADAL] (ADAL) wird automatisch aus dem Azure Active Directory, einschließlich aktualisieren Token erhaltenen Token zwischengespeichert.

ADAL bietet eine standardmäßige token Cache Implementierung. Jedoch dieser token Cache richtet sich an native Client-apps, und ist _nicht_ für Web apps geeignet sind:

-   Es ist eine statische Instanz und nicht Thread-Sicherheit.
-   Es wird nicht an eine große Anzahl von Benutzern, skalieren, da Token von allen Benutzern in der gleichen Wörterbuch wechseln.
-   Es kann nicht in einer Farm Webservern gemeinsam verwendet werden.

Stattdessen sollten Sie einen benutzerdefinierten token Cache, die von der ADAL abgeleitet wird implementieren `TokenCache` Klasse aber eignet sich für eine Server-Umgebung und bietet wünschenswert Grad der Isolation zwischen Token für andere Benutzer.

Die `TokenCache` Klasse speichert ein Wörterbuch von Token, die vom Herausgeber, Ressourcen, Client-ID und Benutzer indiziert. Ein benutzerdefinierter token Cache sollte dieses Wörterbuch in einem Datenträger zur Speicherung, wie etwa einen Redis Cache geschrieben werden.

In der Anwendung Tailspin Umfragen die `DistributedTokenCache` Klasse implementiert token Cache. Diese Implementierung verwendet die [IDistributedCache] [ distributed-cache] Abstraktion von ASP.NET Core 1.0. Auf diese Weise alle `IDistributedCache` Implementierung als einen Datenträger zur Speicherung verwendet werden kann.

-   Standardmäßig wird die app Umfragen einen Redis Cache verwendet.
-   Sie können für eine einzelne Instanz Webserver ASP.NET Core 1.0 [in-Memory-Cache]verwenden[in-memory-cache]. (Dies ist auch eine gute Option für die app während der Entwicklung lokal ausgeführt wird.)

> [AZURE.NOTE] Aktuell wird der Redis Cache für .NET Core nicht unterstützt.

`DistributedTokenCache`Speichert die Cachedaten als Schlüssel/Wert-Paare in der Datenträger zur Speicherung. Der Schlüssel ist die Benutzer-ID sowie die Client-ID, damit der Datenträger zur Speicherung eigener-Cache-Daten für jede eindeutige Kombination von Client-User enthält.

![Token cache](media/guidance-multitenant-identity/token-cache.png)

Der Datenträger zur Speicherung ist durch Benutzer aufgeteilt. Für jede HTTP-Anforderung die Token für diesen Benutzer aus den Datenträger zur Speicherung gelesen und in geladen sind die `TokenCache` Wörterbuch. Wenn Redis als den Datenträger zur Speicherung verwendet wird, jeder Server-Instanz in einer Serverfarm Lesen/Schreiben zum gleichen Cache und dieser Ansatz skaliert für viele Benutzer.

## <a name="encrypting-cached-tokens"></a>Verschlüsseln von zwischengespeicherten Token

Token sind vertrauliche Daten aus, da der Zugriff auf Ressourcen eines Benutzers gewährt werden. (Darüber hinaus keine im Gegensatz zu eines Benutzerkennworts nur einen Hash für das Token gespeichert werden.) Daher ist es entscheidend, Token schützen gefährdet wird. Cache Redis gesicherten durch ein Kennwort geschützt ist, aber wenn jemand das Kennwort erhält, könnte ausgegeben alle zwischengespeicherten Access Token. Aus diesem Grund der `DistributedTokenCache` alle Elemente, die sie in der Datenträger zur Speicherung schreibt verschlüsselt. Erfolgt die Verschlüsselung mit den ASP.NET Core 1.0 [Datenschutz] [ data-protection] APIs.

> [AZURE.NOTE] Wenn Sie auf Azure-Websites bereitstellen, werden die Schlüssel auf Netzwerkspeicher gesichert und auf allen Computern synchronisiert (finden Sie unter [Key Management][key-management]). Standardmäßig Tasten werden nicht verschlüsselt, wenn Websites in Azure ausgeführt, aber Sie können [mit einem x 509-Zertifikat Verschlüsselung aktivieren][x509-cert-encryption].


## <a name="distributedtokencache-implementation"></a>Implementierung DistributedTokenCache

Die [DistributedTokenCache] [ DistributedTokenCache] -Klasse ist von der ADAL [TokenCache] [ tokencache-class] Class.

Im Konstruktor der `DistributedTokenCache` Klasse erstellt einen Schlüssel für den aktuellen Benutzer, und Laden Sie den Cache aus der Datenträger zur Speicherung:

```csharp
public DistributedTokenCache(
    ClaimsPrincipal claimsPrincipal,
    IDistributedCache distributedCache,
    ILoggerFactory loggerFactory,
    IDataProtectionProvider dataProtectionProvider)
    : base()
{
    _claimsPrincipal = claimsPrincipal;
    _cacheKey = BuildCacheKey(_claimsPrincipal);
    _distributedCache = distributedCache;
    _logger = loggerFactory.CreateLogger<DistributedTokenCache>();
    _protector = dataProtectionProvider.CreateProtector(typeof(DistributedTokenCache).FullName);
    AfterAccess = AfterAccessNotification;
    LoadFromCache();
}
```

Die Taste erstellte Verkettung der Benutzer-ID und Kunden-ID an. Diese beiden werden entnommen Ansprüche in des Benutzers gefunden `ClaimsPrincipal`:

```csharp
private static string BuildCacheKey(ClaimsPrincipal claimsPrincipal)
{
    string clientId = claimsPrincipal.FindFirstValue("aud", true);
    return string.Format(
        "UserId:{0}::ClientId:{1}",
        claimsPrincipal.GetObjectIdentifierValue(),
        clientId);
}
```

Zum Laden der Daten zwischengespeichert werden sollen, lesen Sie das serialisierte Blob aus der Datenträger zur Speicherung, und rufen `TokenCache.Deserialize` , das Blob in Cachedaten zu konvertieren.

```csharp
private void LoadFromCache()
{
    byte[] cacheData = _distributedCache.Get(_cacheKey);
    if (cacheData != null)
    {
        this.Deserialize(_protector.Unprotect(cacheData));
    }
}
```

Bei jedem ADAL Zugriff auf den Cache, es wird ausgelöst, eine `AfterAccess` Ereignis. Wenn die Cachedaten geändert hat, die `HasStateChanged` -Eigenschaft true ist. In diesem Fall der Datenträger zur Speicherung die Änderung entsprechend aktualisiert, und legen Sie anschließend `HasStateChanged` auf false.

```csharp
public void AfterAccessNotification(TokenCacheNotificationArgs args)
{
    if (this.HasStateChanged)
    {
        try
        {
            if (this.Count > 0)
            {
                _distributedCache.Set(_cacheKey, _protector.Protect(this.Serialize()));
            }
            else
            {
                // There are no tokens for this user/client, so remove the item from the cache.
                _distributedCache.Remove(_cacheKey);
            }
            this.HasStateChanged = false;
        }
        catch (Exception exp)
        {
            _logger.WriteToCacheFailed(exp);
            throw;
        }
    }
}
```

TokenCache sendet zwei andere Ereignisse an:

- `BeforeWrite`. Unmittelbar vor dem ADAL in den Cache schreibt aufgerufen. Dies können Sie eine Strategie Parallelität implementieren
- `BeforeAccess`. Unmittelbar vor ADAL aus dem Cache liest aufgerufen. Hier können Sie den Cache, um die neueste Version erneut laden.

In diesem Fall entschieden wir nicht auf diese beiden Ereignisse zu behandeln.

- Gleichzeitiger Zugriff letzte Wins zu schreiben. OK, ist, dass Token unabhängig für jeden Benutzer + Client, gespeichert werden, damit ein Konflikt nur passiert, wenn der gleiche Benutzer zwei gleichzeitigen Login Sitzungen hatte.
- Zum Lesen laden wir den Cache für Dokumente bei jeder Anforderung aus. Besprechungsanfragen sind kurzlebige. Wenn der Cache in dieser Zeit geändert wird, wird die nächste Anforderung von den neuen Wert wählen.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [Föderation mit einem Kunden AD FS für mandantenfähigen in Azure-apps][adfs]

<!-- links -->
[ADAL]: https://msdn.microsoft.com/library/azure/jj573266.aspx
[adfs]: guidance-multitenant-identity-adfs.md
[data-protection]: https://docs.asp.net/en/latest/security/data-protection/index.html
[distributed-cache]: https://docs.asp.net/en/latest/fundamentals/distributed-cache.html
[DistributedTokenCache]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.TokenStorage/DistributedTokenCache.cs
[key-management]: https://docs.asp.net/en/latest/security/data-protection/configuration/default-settings.html
[in-memory-cache]: https://docs.asp.net/en/latest/fundamentals/caching.html
[tokencache-class]: https://msdn.microsoft.com/library/azure/microsoft.identitymodel.clients.activedirectory.tokencache.aspx
[x509-cert-encryption]: https://docs.asp.net/en/latest/security/data-protection/implementation/key-encryption-at-rest.html#x-509-certificate
[Teil einer Serie]: guidance-multitenant-identity.md
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
