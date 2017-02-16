<properties
    pageTitle="Synchronisieren von Azure AD verbinden: Grundlegendes zu Benutzern und Kontakten | Microsoft Azure"
    description="Erläutert Benutzern und Kontakten in Azure AD verbinden synchronisieren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Synchronisieren von Azure AD verbinden: Grundlegendes zu Benutzern und Kontakten

Es gibt mehrere verschiedene Ursachen, warum möchten Sie mehrere Active Directory-Gesamtstrukturen haben und es gibt mehrere verschiedene Bereitstellungstopologien, aus. Allgemeine Modelle gehören einer Konto-Ressource Bereitstellung und GAL sync'ed Gesamtstrukturen nach einer Fusionen und Acquisition an. Auch wenn reine Modelle vorhanden sind, Hybridmodelle sind jedoch allgemeine ebenfalls. Die standardmäßige Konfiguration in Azure AD verbinden synchronisieren wird nicht davon ausgegangen ein bestimmtes Modell jedoch abhängig davon, wie Benutzer übereinstimmenden in der Installation Guide ausgewählt wurde, können unterschiedliche Verhaltensweisen beobachtet werden.

In diesem Thema durchlaufen wir wird das Verhalten der Standard-Konfigurations in bestimmten Topologien. Wir werden durch die Konfiguration geleitet und Synchronisierung Regel-Editor können verwendet werden, um die Konfiguration anzuzeigen.

Es gibt einige allgemeine Regeln, die die Konfiguration setzt voraus:

- Unabhängig davon, welcher Reihenfolge wir aus der Quelle Active Directory importieren, sollte das Ergebnis immer identisch sein.
- Ein aktives Konto wird immer Anmeldung, einschließlich **UserPrincipalName** und **SourceAnchor**beteiligen möchten.
- Ein deaktiviertes Konto wird UserPrincipalName und SourceAnchor, beitragen, es sei denn, es eines verknüpften Postfachs, ist es ist keine Aktives Konto gefunden werden.
- Ein Konto mit dem Postfach verknüpfte werden nie für UserPrincipalName und SourceAnchor verwendet werden. Es wird vorausgesetzt, dass ein aktives Konto später gefunden wird.
- Ein Kontakt Objekt möglicherweise Azure AD als Kontakt oder als Benutzer bereitgestellt werden. Sie wissen nicht wirklich, bis alle Quelle Active Directory-Gesamtstrukturen verarbeitet wurden.

## <a name="contacts"></a>Kontakte

Kontakte, die einen Benutzer in einer anderen Gesamtstruktur darstellt, ist häufig nach einer Fusionen und Acquisition, wo eine Lösung GALSync zwei oder mehr Exchange-Gesamtstrukturen bridging. Das Objekt Kontakte ist immer aus den Abstand Verbinder mit Metaverse mit dem e-Mail-Attribut verknüpft wird. Wenn bereits ein Kontakt oder Benutzerobjekts mit der gleichen e-Mail-Adresse ist, werden die Objekte zusammen hinzugefügt. Dies wird in der Regel konfiguriert **In aus dem Active Directory – Kontakt teilnehmen**. Es gibt auch eine Regel namens **In aus dem Active Directory – häufig Kontakt** ein Attribut Griff zu Metaverse Attribut **Quellobjekttyp** mit dem Konstanten **Kontakt**. Diese Regel hat Vorrang sehr niedrig können, wenn alle Benutzerobjekt mit dem gleichen Metaverse-Objekt, und klicken Sie dann auf die Regel verknüpft ist **In aus dem Active Directory – Benutzer allgemeine** wird den Wert Benutzer, die diesem Attribut beitragen. Mit dieser Regel wird dieses Attribut der Kontakt ist kein Benutzer mit JOIN zugeordnet wurde und dem Wert der Benutzer verfügen, wenn mindestens ein Benutzer gefunden wurde.

Für die Bereitstellung ein Objekt Azure AD, erstellt **Out-AAD – teilnehmen wenden Sie sich an** die Regel für ausgehende ein Kontakt Objekt, wenn die Metaverse Attribut **Quellobjekttyp** **wenden Sie sich an**festgelegt ist. Wenn dieses Attribut **Benutzer**festgelegt ist, klicken Sie dann die Regel **Out-AAD – Benutzer teilnehmen an** einem Objekt stattdessen erstellt.
Es ist möglich, dass ein Objekt für die Kundendaten ist von Kontakt in Benutzer beim Weitere Quelle Active Directory importiert und synchronisiert werden.

Beispielsweise werden in einer Suchtopologie GALSync wir finden Contact-Objekte für alle Benutzer in der zweiten Gesamtstruktur beim Importieren von der ersten Gesamtstruktur. Dies wird neuer Kontakt Objekte in der Verbinder AAD bereitgestellt werden. Wenn wir später importieren und Synchronisieren von der zweiten Gesamtstruktur, wir real Benutzer suchen und verknüpfen, um die Objekte vorhandenen. Wir dann Kontakte Objekt im AAD löschen und erstellen Sie einen neuen Benutzerobjekts stattdessen.

Wenn Sie ein Suchtopologie haben, in dem Benutzer und als Kontakte dargestellt, stellen Sie sicher, die Sie auswählen, ob der Benutzer auf das Attribut Mail in der Installation Guide entsprechen. Wenn Sie eine andere Option auswählen, werden Sie eine Reihenfolge abhängige Konfiguration haben. Contact-Objekte werden immer auf das Attribut Mail teilnehmen, aber Benutzerobjekte werden nur auf das Attribut Mail teilnehmen, wenn diese Option, in der Installation Guide ausgewählt wurde. Sie könnten dann einhandeln mit zwei verschiedene Objekte im Metaverse mit dem gleichen e-Mail-Attribut, wenn das Objekt Kontakte vor dem Benutzerobjekt importiert wurde. Während in Azure AD exportieren wird ein Fehler ausgelöst. Dieses Verhalten ist beabsichtigt und weist darauf hin, ungültige Daten oder die Suchtopologie während der Installation nicht ordnungsgemäß identifiziert wurde.

## <a name="disabled-accounts"></a>Deaktivierte Konten

Deaktivierte Konten werden ebenfalls in Azure Active Directory synchronisiert. Deaktivierte Konten sind gemeinsam Ressourcen in Exchange, beispielsweise Konferenzräumen darstellen. Die Ausnahme ist mit dem Postfach verknüpfte Benutzer. wie zuvor erwähnt werden diese nie ein Azure AD-Konto bereitstellen.

Vorgabe wird eine, wenn ein deaktiviertes Benutzerkonto gefunden wird, und klicken Sie dann wir ein anderes Aktives Konto später nicht findet und das Objekt wird nach der Bereitstellung zu Azure AD mit dem UserPrincipalName und SourceAnchor gefunden. Für den Fall, dass eine andere Aktives Konto auf dasselbe Objekt Metaverse hinzugefügt werden soll, werden deren UserPrincipalName und SourceAnchor verwendet werden.

## <a name="changing-sourceanchor"></a>Ändern der sourceAnchor

Wenn ein Objekt in Azure AD exportiert wurde und sie nicht so ändern Sie die SourceAnchor nicht mehr zulässig ist. Wenn das Objekt exportiert wurde wird mit dem **SourceAnchor** Wert von Azure AD akzeptiert die Metaverse Attribut **CloudSourceAnchor** festgelegt. **SourceAnchor** wird geändert und nicht entsprechen **CloudSourceAnchor**, löst die Regel **zu AAD – Benutzer beitreten Out** den Fehler **SourceAnchor Attribut hat sich geändert**. In diesem Fall müssen die Konfiguration oder Daten korrigiert werden, damit die gleichen SourceAnchor vorhanden ist, in dem Metaverse erneut bevor des Objekts erneut synchronisiert werden kann.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure AD verbinden synchronisieren: Anpassen von Optionen für die Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
