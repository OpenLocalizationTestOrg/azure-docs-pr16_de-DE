<properties
    pageTitle="Azure AD-verbinden: Problembehandlung von Fehlern bei der Synchronisierung | Microsoft Azure"
    description="Erläutert, wie Sie beheben aufgetretenen während der Synchronisierung mit Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Problembehandlung von Fehlern bei der Synchronisierung
Identitätsdaten von Windows Server Active Directory (AD DS) mit Azure Active Directory (Azure AD) synchronisiert werden konnte Fehler auftreten. Dieser Artikel enthält eine Übersicht der verschiedenen Arten von Synchronisierungsfehlern, einige möglichen Szenarien, die diesen Fehler und mögliche Methoden zum Beheben der Fehler verursachen. Dieser Artikel enthält die allgemeine Fehlertypen und möglicherweise nicht alle möglichen Fehler behandelt.

 In diesem Artikel wird vorausgesetzt, der Reader befindet sich die zugrunde liegende vertraut [Entwerfen Konzepte Azure AD-und Azure AD-verbinden](active-directory-aadconnect-design-concepts.md).

Mit der neuesten Version von Azure AD verbinden \(August 2016 oder höher\), ein Bericht zu Synchronisierungsfehlern steht im [Azure-Portal](https://aka.ms/aadconnecthealth) als Teil der Azure AD verbinden Gesundheit für synchronisieren.


Starten von 1 September 2016 [Azure Active Directory doppelte Attribut Stabilität](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) Feature wird standardmäßig für alle *neuen* Azure-Active Directory-Mandanten aktiviert werden. Dieses Feature wird automatisch in den kommenden Monaten für vorhandene Mandanten aktiviert sein.

Azure AD verbinden führt 3 Arten von Vorgängen aus den Verzeichnissen synchron behält: Synchronisierung, importieren und exportieren. Fehler können in alle Vorgänge erfolgen. Dieser Artikel befasst sich hauptsächlich auf Fehler beim Exportieren in Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Fehler beim Exportieren in Azure AD
Folgende Abschnitt beschreibt die verschiedenen Typen von Synchronisierungsfehlern, die während des Exportvorgangs in Azure Active Directory mithilfe von Azure AD-Netzwerke auftreten können. Diesen Connector kann durch das Namensformat wird "Contoso. identifiziert werden *onmicrosoft.com*".
Fehler beim Exportieren in Azure AD an, die den Vorgang \(hinzufügen, aktualisieren und löschen usw.\) von Azure AD verbinden versuchten \(synchronisieren-Engine\) auf Azure Active Directory fehlgeschlagen ist.

![Exportieren von Fehlern (Übersicht)](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Konflikt Datenfehlern
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beschreibung
- Wenn Azure AD verbinden \(synchronisieren-Engine\) Azure Active Directory hinzufügen oder Aktualisieren von Objekten angewiesen, Azure AD entspricht eingehenden verwenden das Attribut **SourceAnchor** dem Attribut **ImmutableId** von Objekten in Azure AD-Objekts. Diese Übereinstimmung heißt eine **Harte entsprechen**.
- Wenn Azure AD- **nicht finden kann** jedes Objekt, das die **ImmutableId** entspricht-Attribut mit dem Attribut **SourceAnchor** des eingehenden-Objekts vor der Bereitstellung eines neuen Objekts, greift zurück, wenn Sie die Attribute ProxyAddresses und UserPrincipalName verwenden, um eine Übereinstimmung zu ermitteln. Diese Übereinstimmung heißt eine **Weiche entsprechen**. Die weiche entsprechen sollen Objekte, die mit der neuen Objekte, die während der Synchronisierung hinzugefügt/aktualisiert wird bereits im Azure AD (die in Azure AD Quelle werden) entsprechen, die die betreffende Entität (Benutzer, Gruppen) lokal darstellen.
- **InvalidSoftMatch** -Fehler tritt auf, wenn die harte Entsprechung findet keine, dass übereinstimmende Objekten **und** weiche entsprechen ein übereinstimmendes Objekt findet, aber das Objekt hat einen anderen Wert von *ImmutableId* als das eingehende Objekt *SourceAnchor*, vorgeschlagen wird, das übereinstimmende Objekt mit einem anderen Objekt in lokal Active Directory synchronisiert wurde.

Kurzum, sollte das Objekt mit weiche abgeglichen werden in der Reihenfolge für die weiche Übereinstimmung entwickelt, keiner Wert für die *ImmutableId*verfügen. Wenn ein Objekt mit *ImmutableId* mit festgelegt wird ein Wert weiß nicht die harte Entsprechung jedoch erfüllen das Kriterium weiche-Vergleich der Vorgang ein Synchronisierungsfehler InvalidSoftMatch bedingt.

Zwei oder mehr Objekte den gleichen Wert der folgenden Attribute haben zulässig Azure Active Directory-Schema nicht. \(Dies ist keine vollständige Liste.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- ObjectId

>[AZURE.NOTE] [Azure AD-Attribut duplizieren Attribut Stabilität](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) Feature wird auch als das Standardverhalten der Azure Active Directory Variationswebsites bereitgestellt wird.  Dadurch wird die Anzahl der Synchronisierungsfehler durch Azure AD-verbinden (wie bei anderen Clients synchronisieren) angezeigt, indem Sie Azure AD duplizierten ProxyAddresses und UserPrincipalName Attribute auf lokale AD-Umgebungen teilnehmende ganzer verarbeitet schlechter machen reduziert. Dieses Feature wird der Fehler Duplikate nicht behoben. Damit die Daten weiterhin korrigiert werden müssen. Ermöglicht jedoch die Bereitstellung von neue Objekte die andernfalls blockiert werden aus, die aufgrund doppelte Werte in Azure AD bereitgestellt werden. Dadurch wird die Anzahl der Synchronisierungsfehler zurückgegeben, die an den Client Synchronisierung auch reduziert.
Wenn dieses Feature für Ihren Mandanten aktiviert ist, sehen Sie nicht die InvalidSoftMatch Synchronisierungsfehler während der Bereitstellung neuer Objekte angezeigt.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Beispielszenarien für InvalidSoftMatch
1. Zwei oder mehr Objekte mit demselben Wert von Attribut ProxyAddresses jeglichen lokal Active Directory. Nur von einem ist in Azure AD nach der Bereitstellung abrufen.
2. Zwei oder mehr Objekte mit demselben Wert von UserPrincipalName jeglichen lokal Active Directory. Nur von einem ist in Azure AD nach der Bereitstellung abrufen.
3. Ein Objekt wurde in den auf lokale Active Directory mit dem gleichen Wert ProxyAddresses-Attribut als mit einem vorhandenen Objekt in Azure Active Directory hinzugefügt. Das Objekt lokal hinzugefügt wird nicht in Azure Active Directory bereitgestellt abrufen.
4. Ein Objekt wurde in lokal Active Directory mit demselben Wert von UserPrincipalName Attribut wie ein Azure Active Directory-Konto hinzugefügt. Das Objekt ist nicht in Azure Active Directory bereitgestellt abrufen.
5. Ein synchronisierter Konto aus Gesamtstruktur verschoben wurde A, um Gesamtstruktur b Azure AD verbinden (synchronisieren-Engine) wurde Objekt-GUID-Attribut verwenden, um die SourceAnchor zu berechnen. Nach dem Verschieben Gesamtstruktur unterscheidet der Wert von der SourceAnchor. Das neue Objekt (von A) für die Synchronisierung mit dem vorhandenen Objekt in Azure AD schlägt fehl.
6. Ein Objekt synchronisierten haben versehentlich aus Active Directory und ein neues Objekt wurde in Active Directory erstellt für die betreffende Entität (z. B. Benutzer) ohne Löschen des Kontos in Azure Active Directory lokal gelöscht. Das neue Konto nicht synchronisieren mit den vorhandenen Azure AD-Objekt.
7. Verbinden von Azure AD wurde deinstalliert und neu installiert. Bei der erneuten Installation wurde ein anderes Attribut als die SourceAnchor ausgewählt. Alle Objekte, die zuvor synchronisiert haben nicht mehr synchronisiert InvalidSoftMatch Fehler.

#### <a name="example-case"></a>Groß-/Kleinschreibung Beispiel:
1. **Bob Smith** ist ein synchronisierter Benutzer in Active Directory aus Azure lokal Active Directory *"contoso.com"*
2. Bob Smith **UserPrincipalName** festgelegt ist, als **bobs@contoso.com**.
3. **"Abcdefghijklmnopqrstuv =="** ist der **SourceAnchor** berechnet, indem Sie mit Bob Smith **Objekt-GUID** aus auf Azure AD-Verbinden von Gebäuden Active Directory, welche ist die **ImmutableId** für Bob Smith in Azure Active Directory.
4. Bob verfügt auch über die folgenden Werte für das Attribut **ProxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Ein neuer Benutzer, **Bob Taylor**, wird der auf lokale Active Directory hinzugefügt.
6. Bob Taylors **UserPrincipalName** festgelegt ist, als **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** ist der **SourceAnchor** berechnet, indem Sie mithilfe des Bob Taylor **Objekt-GUID** aus auf Azure AD-Verbinden von Gebäuden Active Directory. Bob Taylors Objekts hat noch nicht zum Azure Active Directory synchronisiert.
8. Bob Taylor weist die folgenden Werte für das Attribut proxyAddresses
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Während der Synchronisierung Azure AD verbinden erkennt das Hinzufügen von Bob Taylor in lokal Active Directory und bitten Sie Azure AD in die gleiche Änderung vornehmen.
10. Azure AD wird harte Entsprechung zuerst ausgeführt werden. D. h., es ist es jedes Objekt mit dem ImmutableId gleich suchen wird "abcdefghijkl0123456789 ==". Harte Entsprechung schlägt fehl, kein anderes Objekt in Azure AD wurde, sind die ImmutableId.
11. Azure AD versucht, Weiche-Bob Taylor Übereinstimmung. D. h., suchen es, wenn die drei Werte, einschließlich mit Objekten mit ProxyAddresses vorhanden istsmtp:bob@contoso.com
12. Azure AD findet Bob Smith Objekt den weiche-Match Kriterien entsprechen. Dieses Objekt hat aber den Wert ImmutableId = "Abcdefghijklmnopqrstuv ==". Was bedeutet, dass dieses Objekt wurde von einem anderen Objekt in lokal Active Directory synchronisiert. Auf diese Weise Azure AD weiche-Übereinstimmung kann nicht dieser Objekte und führt zu Fehlern im ein Synchronisierungsfehler **InvalidSoftMatch** .

#### <a name="how-to-fix-invalidsoftmatch-error"></a>So InvalidSoftMatch Fehler zu beheben
Die häufigste Ursache für den Fehler InvalidSoftMatch ist zwei Objekte mit anderen SourceAnchor \(ImmutableId\) müssen den gleichen Wert für den ProxyAddresses und/oder UserPrincipalName Attributen, die während der weiche-Match auf Azure AD verwendet werden. Um die Übereinstimmung mit ungültigen weiche beheben

1.  Benennen Sie den duplizierten ProxyAddresses, UserPrincipalName oder andere Attributwert, der den Fehler verursacht. Auch die zwei identifizieren \(mindestens\) Objekte sind sind davon betroffen. Der Bericht durch [Azure AD verbinden Gesundheit für synchronisieren](https://aka.ms/aadchsyncerrors) können Sie die beiden Objekte leichter identifizieren.
2. Welches Objekt duplizierten Wert haben fortgesetzt werden soll, und welche Objekt nicht sollte zu identifizieren.
3. Entfernen Sie den duplizierten Wert aus das Objekt, das keine dieser Wert ein. Beachten Sie, dass Sie die Änderung im Verzeichnis vornehmen sollten, wo das Objekt aus Quelle ist. In einigen Fällen müssen Sie eines der Objekte in Konflikt löschen.
4. Wenn Sie in der auf lokale AD die Änderung vorgenommen haben, lassen Sie Azure AD verbinden die Änderung zu synchronisieren.

Beachten Sie, dass synchronisieren Fehlerbericht in Azure AD verbinden Gesundheit für synchronisieren alle 30 Minuten aktualisiert wird, und der Fehler aus der neuesten Synchronisierungsversuch enthält.

>[AZURE.NOTE] ImmutableId, sollten nicht in die Gültigkeitsdauer des Objekts per Definition ändern. Wenn Azure AD-Verbinden mit einige Szenarien, denken Sie daran, in der vorstehenden Liste konfiguriert wurde, Sie konnte einhandeln in einer Situation Azure AD verbinden berechnet, in einen anderen Wert, der die SourceAnchor für die AD-Objekt, das die gleiche darstellt Entität (gleichen Benutzer/Gruppe/Kontakt usw.), die ein vorhandenes Azure AD-Objekt enthält, die Sie weiterhin verwenden möchten.

#### <a name="related-articles"></a>Verwandte Artikel
- [Verhindern, dass doppelte oder ungültige Attribute Verzeichnissynchronisation in Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beschreibung
Azure AD versucht, zwei Objekte weiche entsprechen, es ist möglich, dass zwei Objekte verschiedener "Typ" (z. B. Benutzer, Gruppe, Kontakt usw. Objekt) dieselben Werte für den Attributen, die zum Ausführen der weichen Übereinstimmung verwendet haben. Wie diese Attribute doppelter in Azure AD nicht zulässig ist, kann der Vorgang "ObjectTypeMismatch" Synchronisierungsfehler ergeben.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Beispielszenarien für ObjectTypeMismatch zurück
- In Office 365 wird eine e-Mail-aktivierten Sicherheitsgruppe erstellt. Administrator fügt einen neuen Benutzer oder Kontakt in lokal AD (die zu Azure AD noch nicht synchronisiert) mit dem gleichen Wert für das Attribut ProxyAddresses als mit der Office 365-Gruppe hinzu.

#### <a name="example-case"></a>Beispiel für Groß-/Kleinschreibung

1. Administrator erstellt eine neue e-Mail-aktivierten Sicherheitsgruppe in Office 365 für die Abteilung steuern und stellt eine e-Mail-Adresse als tax@contoso.com. Dadurch wird das Attribut ProxyAddresses für diese Gruppe mit dem Wert der zugewiesen.**smtp:tax@contoso.com**
2. Ein neuer Benutzer eintritt Contoso.com und ein Konto für den Benutzer mit der ProxyAddress als lokal erstellt wird**smtp:tax@contoso.com**
3. Wenn Azure AD Verbinden des neuen Benutzerkontos synchronisieren wird, erhalten sie den Fehler "ObjectTypeMismatch".

#### <a name="how-to-fix-objecttypemismatch-error"></a>So ObjectTypeMismatch Fehler zu beheben
Die häufigste Ursache für den Fehler ObjectTypeMismatch ist, dass zwei Objekte unterschiedlichen Typs (Benutzer, Gruppe, Kontakt usw.) den gleichen Wert für das Attribut ProxyAddresses haben. Um die ObjectTypeMismatch zu beheben:

1.  Identifizieren der duplizierten ProxyAddresses (oder ein anderes Attribut) Wert, der den Fehler verursacht. Auch die zwei identifizieren \(mindestens\) Objekte sind sind davon betroffen. Der Bericht durch [Azure AD verbinden Gesundheit für synchronisieren](https://aka.ms/aadchsyncerrors) können Sie die beiden Objekte leichter identifizieren.
2. Welches Objekt duplizierten Wert haben fortgesetzt werden soll, und welche Objekt nicht sollte zu identifizieren.
3. Entfernen Sie den duplizierten Wert aus das Objekt, das keine dieser Wert ein. Beachten Sie, dass Sie die Änderung im Verzeichnis vornehmen sollten, wo das Objekt aus Quelle ist. In einigen Fällen müssen Sie eines der Objekte in Konflikt löschen.
4. Wenn Sie in der auf lokale AD die Änderung vorgenommen haben, lassen Sie Azure AD verbinden die Änderung zu synchronisieren. Synchronisieren Fehlerbericht in Azure AD verbinden Gesundheit für synchronisieren Ruft alle 30 Minuten aktualisiert und der Fehler aus der neuesten Synchronisierungsversuch enthält.


## <a name="duplicate-attributes"></a>Duplizieren von Attributen
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beschreibung
Zwei oder mehr Objekte den gleichen Wert der folgenden Attribute haben zulässig Azure Active Directory-Schema nicht. Dies ist, dass jedes Objekt in Azure AD erzwungen wird einen eindeutigen Wert, der diese Attribute bei einer bestimmten Instanz haben.

- ProxyAddresses
- UserPrincipalName

Wenn Azure AD verbinden versucht, ein neues Objekt hinzufügen oder aktualisieren Sie ein vorhandenes Objekt mit einem Wert für die oben angegebenen Attribute, der bereits auf ein anderes Objekt in Azure Active Directory zugewiesen, wird der Vorgang "AttributeValueMustBeUnique" Synchronisierungsfehler ergeben.
#### <a name="possible-scenarios"></a>Mögliche Szenarien:
1. Doppelter Wert wird zugewiesen bereits synchronisierten Objekt, das mit einem anderen synchronisierten Objekt in Konflikt steht.

#### <a name="example-case"></a>Groß-/Kleinschreibung Beispiel:
1. **Bob Smith** ist ein synchronisierter Benutzer in Active Directory aus Azure lokal Active Directory "contoso.com"
2. Bob Smith **UserPrincipalName** lokal festgelegt ist, als **bobs@contoso.com**.
3. Bob verfügt auch über die folgenden Werte für das Attribut **ProxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Ein neuer Benutzer, **Bob Taylor**, wird der auf lokale Active Directory hinzugefügt.
5. Bob Taylors **UserPrincipalName** festgelegt ist, als **bobt@contoso.com**.
6. **Bob Taylor** weist die folgenden Werte für das Attribut **ProxyAddresses** i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylors Objekts wird erfolgreich mit Azure Active Directory synchronisiert.
8. Administrator entschieden Bob Taylors **ProxyAddresses** -Attribut mit dem folgenden Wert zu aktualisieren: ich. **smtp:bob@contoso.com**
9. Azure AD versucht Bob Taylors Objekts in Azure AD mit den oben angegebenen Wert aktualisiert, aber, dass Vorgang als fehlschlägt, dass Wert ProxyAddresses "AttributeValueMustBeUnique" Fehler resultierender Bob Smith, bereits zugewiesen ist.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>So AttributeValueMustBeUnique Fehler zu beheben
Die häufigste Ursache für den Fehler AttributeValueMustBeUnique ist zwei Objekte mit anderen SourceAnchor \(ImmutableId\) denselben Wert für die Attribute ProxyAddresses und/oder UserPrincipalName haben. Um AttributeValueMustBeUnique Fehler zu beheben

1.  Benennen Sie den duplizierten ProxyAddresses, UserPrincipalName oder andere Attributwert, der den Fehler verursacht. Auch die zwei identifizieren \(mindestens\) Objekte sind sind davon betroffen. Der Bericht durch [Azure AD verbinden Gesundheit für synchronisieren](https://aka.ms/aadchsyncerrors) kann zwei Objekte identifizieren helfen.
2. Welches Objekt duplizierten Wert haben fortgesetzt werden soll, und welche Objekt nicht sollte zu identifizieren.
3. Entfernen Sie den duplizierten Wert aus das Objekt, das keine dieser Wert ein. Beachten Sie, dass Sie die Änderung im Verzeichnis vornehmen sollten, wo das Objekt aus Quelle ist. In einigen Fällen müssen Sie eines der Objekte in Konflikt löschen.
4. Wenn Sie in der auf lokale AD die Änderung vorgenommen haben, lassen Sie Azure AD verbinden die Änderung für den Fehler behoben erhalten synchronisieren.

#### <a name="related-articles"></a>Verwandte Artikel
-[Verhindern, dass doppelte oder ungültige Attribute Verzeichnissynchronisation in Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Überprüfungsfehler in Daten
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beschreibung
Azure-Active Directory erzwingt verschiedene Einschränkungen auf die Daten selbst, bevor Sie die Daten in dem Verzeichnis geschrieben werden soll. Dies ist, um sicherzustellen, dass Endbenutzer erhalten, die am besten Erfahrung beim Verwenden der Anwendung, die diese Daten abhängig sind.
#### <a name="scenarios"></a>Szenarien
ein. Der UserPrincipalName Attributwert enthält ungültige oder nicht unterstütztes Zeichen.
b. Das Attribut UserPrincipalName folgt nicht das erforderliche Format.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>So IdentityDataValidationFailed Fehler zu beheben

ein. Stellen Sie sicher, dass das Attribut UserPrincipalName Zeichen und erforderliche Format unterstützt hat.

#### <a name="related-articles"></a>Verwandte Artikel
- [Vorbereiten der Benutzer über die verzeichnissynchronisierung zu Office 365-Bereitstellung]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Beschreibung
Dies ist ein spezieller Fall, das ein Synchronisierungsfehler **"DataValidationFailed"** ergibt Wenn des Suffixes UserPrincipalName eines Benutzers aus einer verbunddomäne in einer anderen verbunddomäne geändert wird.

#### <a name="scenarios"></a>Szenarien
Für einen synchronisierte Benutzer wurde das Suffix UserPrincipalName aus einer verbunddomäne in eine andere verbunddomäne lokal geändert. Beispielsweise *UserPrincipalName = bob@contoso.com * wurde geändert in *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Beispiel
1. Bob Smith, ein Konto für Contoso.com wird als einen neuen Benutzer in Active Directory mit UserPrincipalName hinzugefügt.bob@contoso.com
2. Bob zu anderen Division von Contoso.com namens Fabrikam.com verschiebt und seine UserPrincipalName zu geändert wirdbob@fabrikam.com
3. Contoso.com und fabrikam.com Domänen sind partnerverbundkontakte Domänen mit Azure Active Directory.
4. Bobs UserPrincipalName wird nicht aktualisiert abrufen und angegeben, ergibt sich ein "DataValidationFailed" Synchronisierungsfehler angezeigt.

#### <a name="how-to-fix"></a>So beheben
Wenn ein Benutzer UserPrincipalName Suffix, von aktualisiert wurde bob@ **"contoso.com"** , um bob@ **fabrikam.com**, wo **contoso.com** und **fabrikam.com** **Domänen partnerverbundkontakte**sind, führen Sie die folgende Schritte aus, um den Synchronisierungsfehler zu beheben

1. Aktualisieren des Benutzers UserPrincipalName Azure AD aus bob@contoso.com auf bob@contoso.onmicrosoft.com. Den folgenden PowerShell-Befehl können mit dem Azure AD-PowerShell-Modul:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Zulassen des nächsten Synchronisieren Zyklus Synchronisierung versuchen. Kann ich diese Zeit Synchronisierung erfolgreich und UserPrincipalName Bob eine Verbindung aktualisiert wird bob@fabrikam.com wie erwartet.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beschreibung
Wenn ein Attribut der zulässige Größenlimit, maximale Länge oder Anzahl Grenzwert für durch Azure-Active Directory-Schema überschreitet, wird der Vorgang Synchronisation Synchronisierungsfehler **LargeObject** oder **ExceededAllowedLength** ergeben. In der Regel auftritt dieser Fehler, für den folgenden Attributen

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Mögliche Szenarien
1. Bobs UserCertificate-Attribut ist zu viele Zertifikate zugewiesen an Bob speichern. Diese können ältere, abgelaufene Zertifikate enthalten.
2. Bobs ThmubnailPhoto festlegen in Active Directory ist zu groß, um in Azure Active Directory synchronisiert werden.
3. Während der automatischen Population das Attribut ProxyAddresses in Active Directory, ein Objekt zugewiesen haben > 500 ProxyAddresses.

### <a name="how-to-fix"></a>So beheben

1. Stellen Sie sicher, dass das Attribut bewirken, dass des Fehlers innerhalb der zulässigen Einschränkung.

## <a name="related-links"></a>Links zu verwandten Themen
- [Suchen Sie Active Directory-Objekte in Active Directory Administrative Center] (https://technet.microsoft.com/library/dd560661.aspx)
- [Vorgehensweise zum Abfragen von Azure Active Directory für ein Objekt mithilfe der Azure-Active Directory-PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
