<properties
    pageTitle="Azure Active Directory Hybrid Identität Entwurf Faktoren - bestimmen kombinierte authentifizierungsanforderungen"
    description="Mit bedingten Access-Steuerelement überprüft Azure Active Directory die bestimmten Bedingungen, die Sie bei der Authentifizierung des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem Sie diese Bedingung erfüllt sind, wird der Benutzer authentifiziert und Zugriff auf die Anwendung zulässig."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Ermitteln Sie mehrstufige authentifizierungsanforderungen für Ihre Identität Hybrid-Lösung

In dieser Welt der Mobilität, mit Benutzern den Zugriff auf Daten und Anwendungen in der Cloud und von jedem Gerät diese Informationen Schutz größter geworden.  Jeden Tag vorliegt eine neue Überschrift zu einer sicherheitsverletzung.  Obwohl es ist keine Garantie für solche Verletzung, enthält mehrstufige Authentifizierung, eine zusätzliche Sicherheitsebene zu verhindern, dass diese Verletzung.
Zunächst die Auswertung erforderlichen Organisationen für die kombinierte Authentifizierung. Das heißt, was die Organisation versucht, gesichert.  Diese Bewertung ist wichtig, die technischen Vorschriften für einrichten und aktivieren die Benutzer Organisationen für kombinierte Authentifizierung definieren.

>[AZURE.NOTE]
Wenn Sie nicht vertraut mit MFA und was bedeutet, es wird dringend empfohlen, dass Sie den Artikel lesen [Neuigkeiten Azure kombinierte Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md) weiterlesen in diesem Abschnitt vor.

Vergewissern Sie sich vor beantworten:

- Versucht Ihr Unternehmen, secure Microsoft apps? 
- Wie dieser apps veröffentlicht werden?
- Bietet Ihr Unternehmen RAS Mitarbeiter apps lokal zugreifen können?

Wenn Ja, welcher Art von remote Access? Sie müssen außerdem ausgewertet werden soll, in dem Benutzer, die diese Applikationen zugreifen gespeichert werden. Diese Bewertung ist ein weiterer wichtiger Schritt in die richtige kombinierte Authentifizierungsstrategie definieren. Vergewissern Sie sich die folgenden Fragen beantworten:

- Wo werden Benutzer? gefunden werden
- Können sie sich an einer beliebigen Stelle befinden?
- Möchte Ihr Unternehmen Einschränkungen entsprechend der Standort des Benutzers herstellen?

Wenn Sie diese Anforderungen verstanden haben, ist es wichtig, auch des Benutzers Anforderungen für kombinierte Authentifizierung ausgewertet werden soll. Diese Bewertung ist wichtig, da sie die Anforderungen für kombinierte Authentifizierung einführen definiert werden. Vergewissern Sie sich die folgenden Fragen beantworten:

- Sind Benutzer mit kombinierte Authentifizierung vertraut?
- Werden einige Verwendungsmöglichkeiten bieten zusätzliche Authentifizierung erforderlich?  
 - Wenn Ja, immer, wenn aus externen Netzwerken oder den Zugriff auf bestimmte Programme oder unter anderen Umständen in Kürze?
- Müssen die Benutzer Schulung zum Einrichten und kombinierte Authentifizierung implementieren?
- Was sind die wichtigsten Szenarien, die Ihr Unternehmen kombinierte Authentifizierung für ihre Benutzer aktivieren möchte?

Nachdem Sie die vorherige-Fragen beantworten, werden Sie zu verstehen, wenn es kombinierte Authentifizierung bereits implementiert lokalen gibt können. Diese Bewertung ist wichtig, die technischen Vorschriften für einrichten und aktivieren die Benutzer Organisationen für kombinierte Authentifizierung definieren. Vergewissern Sie sich die folgenden Fragen beantworten:

- Benötigt Ihr Unternehmen berechtigte Konten mit MFA schützen?
- Benötigt Ihr Unternehmen MFA Compliance Gründen für bestimmte Anwendung aktivieren?
- Benötigt Ihr Unternehmen MFA für alle in Frage kommenden Benutzer diese Anwendung oder nur Administratoren aktivieren?
- Haben Sie müssen immer aktiviert MFA oder nur, wenn der Benutzer außerhalb des Unternehmensnetzwerks angemeldet sind?


## <a name="next-steps"></a>Nächste Schritte
[Definieren einer Hybrid Identität-Einführung](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Siehe auch
[Entwurf Aspekte (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
