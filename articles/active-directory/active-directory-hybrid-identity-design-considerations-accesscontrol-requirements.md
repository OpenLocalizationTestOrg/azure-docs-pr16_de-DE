
<properties
    pageTitle="Azure Active Directory Hybrid Identität Entwurf Faktoren - bestimmen Access Steuerelement Anforderungen | Microsoft Azure"
    description="Behandelt die Säulen Identität und Access-Anforderungen für Ressourcen für Benutzer in einer hybridumgebung zu identifizieren."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Ermitteln Sie Access-Steuerelement Anforderungen für Ihre Identität Hybrid-Lösung
Beim Entwerfen einer Organisation ihre Identität Hybrid-Lösung können diese auch diese Gelegenheit nutzen, Access Anforderungen für die Ressourcen zu überprüfen, die sie planen für Benutzer zur Verfügung stellen. Zugreifen auf die Daten cross alle vier Säulen von Identität, die sind:

- Verwaltung
- Authentifizierung
- Autorisierung
- Überwachung

Die Abschnitte, die folgt behandelt Authentifizierung und Autorisierung in weitere Details, Verwaltung und Überwachung sind Teil des Lebenszyklus Hybrid Identität. Lesen Sie weitere Informationen zu diesen Funktionen [ermitteln Hybrid Identität Verwaltungsaufgaben](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) .

>[AZURE.NOTE]
Lesen Sie weitere Informationen zu den einzelnen für diese Säulen [Der vier Säulen der Identität - Identitätsmanagement in ALTER Hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) ein.

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung
Es gibt verschiedene Szenarios für die Authentifizierung und Autorisierung, diese Szenarios werden bestimmte Anforderungen, die von der Hybrid Identität Lösung, die das Unternehmen vor sich geht erfüllen muss getroffen haben. Szenarien mit Business-to-Business (B2B) Kommunikation können eine zusätzliche Herausforderung für IT-Administratoren hinzugefügt werden, da sie benötigen werden, um sicherzustellen, dass die von der Organisation verwendete Methode zur Authentifizierung und Autorisierung mit ihren Partnern Business kommunizieren kann. Während der Prozess Erstellen eines Konzepts für Authentifizierung und Autorisierung Anforderungen stellen Sie sicher, dass die folgenden Fragen beantwortet werden:

- Wird Ihre Organisation authentifizieren und Autorisieren von nur Benutzer, die ihre Identität Management-System am?
 - Gibt es Pläne für B2B-Szenarien?
 - Wenn Ja, wissen Sie bereits, welche Protokolle (SAML, OAuth, Kerberos, Token oder Zertifikate) Verbindung beide Unternehmen verwendet werden?
- Unterstützt der Hybrid Identität Lösung, die Sie beabsichtigen, Support übernehmen diese Protokolle?

Ein anderer wichtiger Punkt zu berücksichtigen ist, in dem das Authentifizierung Repository, das von Benutzern und Partnern verwendet werden gespeichert werden und das administrative Modell verwendet werden. Beachten Sie die folgenden zwei Core Optionen aus:
- Zentralisierte: in diesem Modell der Anmeldeinformationen des Benutzers, Richtlinien und Verwaltung können werden in den lokalen oder in der Cloud.
- Hybrid: in diesem Modell Anmeldeinformationen, Richtlinien und Verwaltung des Benutzers in den lokalen und eine repliziert in der Cloud werden.

Welches Modell Ihrer Organisation übernommen werden variiert entsprechend ihren geschäftlichen Anforderungen, die Sie beantworten Sie die folgenden Fragen zum Identifizieren der Stelle, an der das Identität Management-System gespeichert werden soll, und der Administrator-Modus verwenden möchten:

- Wird Ihre Organisation aktuell eine Identitätsmanagement lokalen?
 - Wenn Ja, möchten sie behalten?
 - Gibt es erforderlichen Rechts- oder Compliance an, dass Ihre Organisation die bestimmt folgen, in denen das Identität Management-System befinden soll?
- Wird Ihre Organisation einmaliges Anmelden für ansässig apps lokal oder in der Cloud verwendet?
 - Wenn Ja, wirkt sich die Annahme eines Modells Identität Hybrid dieses Verfahren?

## <a name="access-control"></a>Steuerung des Benutzerzugriffs
Während der Authentifizierung und Autorisierung Hauptelemente zum Aktivieren des Zugriffs auf Unternehmensdaten durch Überprüfung des Benutzers sind, ist es auch wichtig, die Zugriffsebene steuern, die diese Benutzer haben und die Ebene der Access-Administratoren haben über die Ressourcen, die sie verwalten. Ihre Identität Hybrid-Lösung muss präzise Zugriff auf Ressourcen, Delegierung und Rolle Basis Access-Steuerelement bereitstellen können. Stellen Sie sicher, dass die folgende Frage zur Steuerung des Benutzerzugriffs beantwortet werden:

- Verfügt Ihr Unternehmen mehr als ein Benutzer mit erhöhten Berechtigungen zur Verwaltung Ihres Systems Identität?
 - Wenn Ja, benötigt jeder Benutzer die gleiche Zugriffsebene?
- Müssten Ihres Unternehmens den Zugriff auf Benutzer zum Verwalten von bestimmter Ressourcen übertragen?
 - Wenn Ja, geschieht, wie häufig dies?
- Müssten Ihres Unternehmens integrieren Funktionen von Access-Steuerelements zwischen lokalen und Cloud Ressourcen?
- Beschränken Sie den Zugriff auf Ressourcen nach bestimmten Umständen Ihr Unternehmen muss würde?
- Würde Ihr Unternehmen eine Anwendung vorliegt, die benutzerdefinierte Steuern des Zugriffs auf einige Ressourcen?
 - Wenn Ja, wo diese apps befinden (lokal oder in der Cloud)?
 - Wenn Ja, wo sind die adressieren Ressourcen ansässig (lokal oder in der Cloud)?

>[AZURE.NOTE]
Vergewissern Sie sich zum Erfassen von Notizen für jede Antwort und die Gründe für die Antwort zu verstehen. [Definieren von Strategie für den Datenschutz](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) wird über die verfügbaren Optionen und vor-und Nachteile der einzelnen Optionen wechseln.  Indem Sie diese Fragen beantworten müssen Sie auswählen, welche Option am besten Bedürfnissen Ihres Unternehmens passt.

## <a name="next-steps"></a>Nächste Schritte

[Ermitteln der Reaktion Anforderungen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Siehe auch
[Entwurf Aspekte (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
