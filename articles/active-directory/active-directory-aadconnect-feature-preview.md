<properties
   pageTitle="Azure AD-verbinden: Features in der Vorschau | Microsoft Azure"
   description="In diesem Thema werden in weitere Details-Features, die in der Vorschau in Azure AD verbinden sind."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Weitere Informationen zu den Funktionen, die in der Vorschau
In diesem Thema werden Informationen zum Verwenden der Features derzeit in der Vorschau an.

## <a name="group-writeback"></a>Gruppe abgeschlossenen writebackvorgängen
Die Option für die Gruppe abgeschlossenen writebackvorgängen in optionale Features ermöglicht zu abgeschlossenen writebackvorgängen **Office 365-Gruppen** zu einer Gesamtstruktur mit Exchange installiert Ihnen. Dies ist eine Gruppe, die in der Cloud immer beherrschen ist. Wenn Sie Exchange lokalen haben, und klicken Sie dann Sie diesen Gruppen wieder zu lokalen schreiben können also Benutzer mit einem lokalen Exchange-Postfach können senden und empfangen e-Mails aus diesen Gruppen.

Weitere Informationen zu Office 365-Gruppen und deren Verwendung finden Sie [hier](http://aka.ms/O365g).

Diese Gruppe wird als eine Verteilergruppe im lokalen dargestellt werden AD DS. Der lokalen Exchange-Server muss auf Exchange 2013 Kumulatives Update 8 (herausgegeben im März 2015) oder Exchange 2016 diese neue Gruppentyp zu erkennen.

**Notizen während der Vorschau**

- Adressbuch-Attribut der Adresse wird derzeit nicht in der Vorschau aufgefüllt. Ohne dieses Attribut wird die Gruppe in der globalen Adressliste nicht angezeigt. Die einfachste Möglichkeit zum Auffüllen dieses Attribut besteht darin, verwenden das Exchange-PowerShell-Cmdlet `update-recipient`.
- Nur Gesamtstrukturen mit dem Exchange-Schema sind gültige Ziele für Gruppen. Wenn Sie kein Exchange nicht erkannt wurde, wird Gruppe abgeschlossenen writebackvorgängen nicht möglich, aktivieren werden.
- Nur einzelnen Gesamtstruktur Exchange-Organisation Bereitstellungen werden derzeit unterstützt. Wenn Sie mehrere Exchange-Organisation lokal verwenden, dann benötigen Sie eine lokale GALSync Lösung für diesen Gruppen in der anderen Gesamtstrukturen angezeigt werden.
- Die Gruppe abgeschlossenen writebackvorgängen Features behandelt derzeit nicht Sicherheitsgruppen oder Verteilergruppen.

>[AZURE.NOTE] Ein Azure AD Premium-Abonnement ist für die Gruppe abgeschlossenen writebackvorgängen erforderlich.

## <a name="user-writeback"></a>Benutzer abgeschlossenen writebackvorgängen
> [AZURE.IMPORTANT] Das Feature abgeschlossenen writebackvorgängen Vorschau entfernt wurde in den August 2015 aktualisieren Azure AD verbinden. Wenn Sie es aktiviert haben, sollten Sie dieses Feature deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
Fortsetzen der [benutzerdefinierte Installation von Azure AD verbinden](./connect/active-directory-aadconnect-get-started-custom.md).

Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).
