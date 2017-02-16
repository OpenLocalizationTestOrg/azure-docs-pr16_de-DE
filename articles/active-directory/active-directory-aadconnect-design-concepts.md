<properties
   pageTitle="Azure AD-verbinden: Entwerfen Konzepte | Microsoft Azure"
   description="In diesem Thema werden bestimmte Bereiche der Implementierung Entwurf"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD-verbinden: Design-Konzepte
In diesem Thema dient Bereiche beschrieben, die während des Entwurfs Implementierung Azure AD verbinden über verstanden werden müssen. In diesem Thema wird eine detaillierte technische Informationen auf bestimmte Bereiche und diese Konzepte werden in anderen Themen auch kurz beschrieben.

## <a name="sourceanchor"></a>sourceAnchor
Das Attribut SourceAnchor wird als *unveränderlich während der Lebensdauer eines Objekts Attribut*definiert. Er identifiziert eindeutig als die gleichen Objekt lokal und in Azure AD-Objekt. Das Attribut wird auch als **ImmutableId** bezeichnet, und die beiden Namen austauschbare verwendet werden.

Unveränderlich, das Wort, das ist "kann nicht geändert werden", ist wichtig, die in diesem Thema. Da dieses Attribut den Wert geändert werden kann, nachdem es festgelegt wurde, ist es wichtig, ein Design auswählen, die Ihrem Szenario unterstützt.

Das Attribut wird für die folgenden Szenarien verwendet:

- Wenn Sie ein neuen synchronisieren-Engine Server erstellt oder nach einem Szenario zur Wiederherstellung nach neu erstellt, links dieses Attribut vorhandenen Objekte in Azure AD mit Objekten lokal.
- Wenn Sie eine nur-Cloud-Identität zu einem Datenmodell in synchronisierten Identität verschieben, kann dieses Attribut Objekte "harte Entsprechung" vorhandenen Objekten in Azure AD mit lokalen Objekten.
- Wenn Sie einen Partnerverbund verwenden, wird dieses Attribut zusammen mit den **UserPrincipalName** um einen Benutzer eindeutig identifizieren klicken Sie dann in der Anspruch verwendet.

In diesem Thema spricht nur über SourceAnchor, in Bezug auf Benutzer. Die gleichen Regeln gelten für alle Objekttypen, aber es ist nur für Benutzer, dass dieses Problem in der Regel ein Problem darstellt.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Eine gute SourceAnchor Attribut auswählen
Der Attributwert muss führen Sie die folgenden Regeln:

- Kleiner als 60 Zeichen lang sein
    - Zeichen, die nicht gerade a-Z, A-Z oder 0-9 codierte und als 3 Zeichen gezählt
- Keine Sonderzeichen enthalten: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ < > () '; : , [ ] " @ _
- Global eindeutig sein muss
- Eine Zeichenfolge, ganze Zahl oder Binary muss sein.
- Sollte nicht auf den Namen des Benutzers, diese Änderungen basieren
- Sollte nicht Groß-/Kleinschreibung beachtet werden und Werte, die Groß-/Kleinschreibung variieren möglicherweise vermeiden
- Sollte zugewiesen werden, wenn das Objekt erstellt wurde

Wenn die ausgewählten SourceAnchor nicht vom Typ "String" ist, dann Azure AD verbinden Base64Encode der Attributwert, um sicherzustellen, dass keine Sonderzeichen angezeigt. Wenn Sie einen anderen Föderation Server als ADFS verwenden, vergewissern Sie sich Ihre Server auch Base64Encode das Attribut können.

Das Attribut SourceAnchor beachtet werden. Der Wert "JohnDoe" ist nicht identisch mit "Johndoe". Sie sollten nicht zwei verschiedene Objekte mit nur einen Unterschied im Fall verfügen aber.

Wenn Sie eine einzelne Gesamtstruktur haben ist lokale, klicken Sie dann das Attribut aus, die Sie verwenden sollten **Objekt-GUID**. Dies ist auch das Attribut bei Verwendung von express-Einstellungen in Azure AD verbinden verwendet und auch das Attribut von DirSync verwendet.

Wenn Sie mehrere Gesamtstrukturen besitzen Benutzer können nicht zwischen und Domänen wechseln, ist **Objekt-GUID** ein guter Attribut auch in diesem Fall verwendet werden soll.

Wenn Sie Benutzer zwischen und Domänen verschieben, müssen finden Sie ein Attribut, die nicht geändert oder verschoben werden kann für den Benutzer beim Verschieben. Eine empfohlene Vorgehensweise ist das eine synthetische Attribut vorstellen. Ein Attribut, das einen Beitrag aufnehmen könnte, die eine GUID aussieht wäre geeignet. Während der Erstellung des Objekts eine neue GUID erstellt und über die auf den Benutzer. Eine Regel für benutzerdefinierte synchronisieren kann in den synchronisieren-Engine-Datenbankserver an diesen Wert basierend auf den **Objekt-GUID** erstellen und aktualisieren Sie das ausgewählte Attribut in ADDS erstellt werden. Wenn Sie das Objekt verschieben möchten, stellen Sie sicher, um auch den Inhalt dieses Werts zu kopieren.

Eine andere Lösung besteht darin, wählen Sie ein vorhandenes Attribut aus, die, das Sie kennen nicht geändert wird. Häufig verwendete Attribute umfassen **EmployeeID**. Wenn Sie ein Attribut in Betracht, die Buchstaben enthält ziehen, stellen Sie sicher, es, dass keine Chance, der Groß-/Kleinschreibung (Großbuchstabe im Vergleich zu Kleinbuchstaben) für den Wert für das Attribut ändern kann. Fehlerhafte Attributen, die nicht verwendet werden soll schließen diese Attribute mit dem Namen des Benutzers ein. In einer Hochzeit oder Scheidung wird der Name erwartet ändern möchten, die für dieses Attribut nicht zulässig. Dies ist auch ein Grund, warum Attribute wie **UserPrincipalName**, **e-Mail**und **TargetAddress** nicht auch möglich, wählen Sie den Assistenten zum Installieren von Azure AD verbinden sind. Diese Attribute auch enthalten die @-character, die wird in der SourceAnchor nicht zulässig.

### <a name="changing-the-sourceanchor-attribute"></a>Ändern das Attribut sourceAnchor
Der Wert der SourceAnchor-Attribut kann nicht geändert werden, nachdem das Objekt in Azure Active Directory erstellt wurde, und die Identität ist synchronisiert.

Aus diesem Grund gelten die folgenden Einschränkungen Azure AD verbinden:

- Das Attribut SourceAnchor kann nur bei der ersten Installation festgelegt werden. Wenn Sie den Installationsassistenten erneut ausführen, ist diese Option schreibgeschützt. Wenn Sie diese Einstellung ändern müssen, müssen Sie deinstallieren und erneut zu installieren.
- Wenn Sie einen anderen Azure AD verbinden Server installiert haben, müssen Sie das gleiche wie zuvor verwendete SourceAnchor-Attribut auswählen. Wenn Sie zur Azure AD verbinden wechseln und haben zuvor DirSync verwenden, müssen Sie **Objekt-GUID** verwenden, da dieser das Attribut von DirSync verwendet wird.
- Wenn der Wert für SourceAnchor nach geändert wird wurde das Objekt synchronisieren löst einen Fehler und kann nicht mehr Änderungen auf, dass das Objekt, bevor Sie dieses Problem behoben wurde, und die SourceAnchor ändert sich wieder im Source-Verzeichnis in Azure AD, dann Azure AD verbinden exportiert.

## <a name="azure-ad-sign-in"></a>Azure AD-anmelden
Beim Integrieren von Ihrem lokalen Verzeichnis mit Azure AD, es ist wichtig zu verstehen, wie die synchronisierungseinstellungen den Benutzer Weise auswirken können authentifiziert. Azure AD wird UserPrincipalName (Benutzerprinzipalnamen) zum Authentifizieren des Benutzers verwendet. Wenn Sie Ihre Benutzer synchronisieren, müssen Sie das Attribut für UserPrincipalName Wert verwendet werden soll, zeigen Sie mit auswählen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Das Attribut für UserPrincipalName auswählen
Bei der Auswahl des Attributs für die Bereitstellung von sollte der Wert des UPN in Azure eine verwendet werden sicherzustellen.

- Das Attributwerte entsprechen die Benutzerprinzipalnamen-Syntax (RFC 822), wird angenommen wird des Formatsusername@domain
- Das Suffix Werte entspricht einem überprüft benutzerdefinierte Domänen in Azure AD

In den express-Einstellungen ist die angenommene Wahl für das Attribut UserPrincipalName. Wenn das Attribut UserPrincipalName nicht den Wert enthält Sie Ihre Benutzer bei Azure anmelden möchten, dann müssen Sie **Benutzerdefinierte Installation**auswählen.

### <a name="custom-domain-state-and-upn"></a>Benutzerdefinierte Domänenstatus und Benutzerprinzipalnamen
Es ist wichtig, um sicherzustellen, dass eine Domäne überprüfte, für das UPN-Suffix vorhanden ist.

Johann ist ein Benutzer auf "contoso.com". Johann lokalen Benutzerprinzipalnamen verwenden soll john@contoso.com Azure anmelden, nachdem Sie Benutzer zu Ihrer Azure AD-Verzeichnis contoso.onmicrosoft.com synchronisiert haben. Dazu müssen Sie hinzufügen und überprüfen "contoso.com" als eine benutzerdefinierte Domäne in Azure AD, bevor Sie beginnen können, die Benutzer synchronisieren. Wenn das UPN-Suffix Johann, beispielsweise "contoso.com", keine Domäne überprüfte in Azure AD entsprechen, werden Azure AD das UPN-Suffix mit contoso.onmicrosoft.com ersetzt.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Nicht geroutet lokalen Domänen und UPN für Azure AD
Einige Organisationen haben nicht geroutet Domänen, wie contoso.local oder einfaches Etikett Domänen wie "Contoso". Sie können nicht überprüfen eine Domäne nicht geroutet in Azure Active Directory. Azure AD verbinden kann nur einer überprüft Domäne in Azure AD synchronisieren. Wenn Sie ein Verzeichnis Azure AD-erstellen, wird eine geroutet Domäne, die Standarddomäne für Ihre Azure Anzeige beispielsweise contoso.onmicrosoft.com wird erstellt. Daher ist es erforderlich, alle anderen geroutet Domäne in diesem Fall vergewissern Sie sich für den Fall, dass Sie nicht mit der standardmäßigen onmicrosoft.com-Domäne synchronisieren möchten.

Lesen Sie weitere Informationen über das Hinzufügen und Überprüfung von Domänen [Ihren benutzerdefinierten Domänennamen zu Azure Active Directory hinzufügen](active-directory-add-domain.md) .

Verbinden von Azure AD erkennt, wenn Sie in einer Domäne nicht geroutet Umgebung ausführen und ordnungsgemäß warnen würde aus vertraut anstehen mit express-Einstellungen. Wenn Sie in einer Domäne nicht geroutet befinden, ist es wahrscheinlich, dass der Benutzerprinzipalnamen der Benutzer nicht geroutet Suffixe zu haben. Wenn Sie unter contoso.local ausgeführt werden, schlägt beispielsweise Azure AD-verbinden Sie benutzerdefinierte Einstellungen anstelle der Verwendung von express-Einstellungen verwenden. Verwenden von benutzerdefinierten Einstellungen, können Sie das Attribut angeben, das als UPN verwendet werden soll, bei Azure anmelden können, nachdem die Benutzer auf Azure Active Directory synchronisiert werden.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).
