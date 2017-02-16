
<properties
    pageTitle="Azure Active Directory Hybrid Identität Entwurf Faktoren - bestimmen Vorfall rResponse Anforderungen | Microsoft Azure-Anforderungen "
    description="Bestimmen, Überwachung und Berichterstattung Funktionen für die Hybrid Identität Lösung, die auf dem Computer können IT Ausführen von Aktionen zu identifizieren und Beheben von einer potenziellen Risiken"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Ermitteln der Reaktion Anforderungen für Ihre Identität Hybrid-Lösung

Große oder mittelständische Unternehmen wahrscheinlich haben ein [Vorfall Sicherheitsantwort](https://technet.microsoft.com/library/cc700825.aspx) tragen dazu IT Aktionen entsprechend der Ebene der Vorfall. Das Identität Management-System ist eine wichtige Komponente im Prozess Vorfall Antwort, da es verwendet werden kann, können Sie zu identifizieren, die eine bestimmte Aktion gegenüber dem Ziel durchgeführt. Die Hybrid Identität Lösung muss Möglichkeit zu Überwachung und Berichterstattung Funktionen bieten, die durch genutzt werden können, IT zu identifizieren und Beheben von einer Bedrohungspotenzials Aktionen ausführen. In einen Plan zur Reaktion auf typische müssen Sie die folgenden Phasen als Teil der Plan:

1.  Erste Beurteilung.
2.  Vorfall Kommunikation.
3.  Steuerelement Schaden und Risiken reduzieren.
4.  Was Identifikationen Kompromisse und schwere war.
5.  Beweisen erhalten.
6.  Benachrichtigung an den entsprechenden Parteien.
7.  Wiederherstellung des Systems.
8.  Dokumentation.
9.  Schaden und Kosten Bewertung.
10. Die Revisionsnummer Prozess und planen.

Während die Kennung des welche It wurde Kompromisse und schwere-Phasen, zum Identifizieren der Systeme, die gefährdet, Dateien, die auf die zugegriffen wurde und Bestimmen der Vertraulichkeit dieser Dateien erforderlich. Ihre Identität Hybrid-System sollte möglicherweise erfüllen diese Anforderungen zu unterstützen Sie die Identifizierung des Benutzers, die diese Änderungen vorgenommen. 

## <a name="monitoring-and-reporting"></a>Überwachung und Berichterstattung
Oft können die Identität auch in der ersten Beurteilung Phase Hilfesystem hauptsächlich, wenn das System erstellt hat, in die Überwachung und reporting-Funktionen. Während der anfänglichen Bewertung IT-Administrator muss einen verdächtigen Aktivitäten zu identifizieren, oder sollte das System zum Auslösen eines Vorgangs vorkonfiguriertes automatisch zugrunde liegenden kann. Viele Aktivitäten könnte eine mögliche Angriffen, hinweisen, jedoch in anderen Fällen ein fehlerhaft konfiguriertes System zu einer Reihe falscher Alarme in ein Eindringungserkennungssystem führen kann. 

Das Identität Managementsystem sollte IT-Administratoren identifizieren und melden diese verdächtigen Aktivitäten unterstützen. Normalerweise können diese technischen Anforderungen erfüllt werden, indem Sie alle Systeme für die Überwachung und reporting-Funktionen, die potenzielle Risiken hervorheben kann Probleme. Mithilfe der folgenden Fragen können Sie Ihre Identität Lösung Hybrid, während der Berücksichtigung der Aspekte Reaktion Anforderungen entwerfen:

- Verfügt Ihr Unternehmen Reaktionen Sicherheit Vorfall direkte?
 - Wenn Ja, wird das aktuelle Identität Managementsystem als Teil des Prozesses verwendet?
- Benötigt Ihr Unternehmen verdächtigen anmelden Versuche von Benutzern auf verschiedenen Geräten identifizieren?
- Benötigt Ihr Unternehmen Anmeldeinformationen des betroffenen Benutzers potenzieller erkennen?
- Benötigt Ihr Unternehmen zu überwachender Access und die Aktion des Benutzers?
- Benötigt Ihr Unternehmen wissen, wann ein Benutzer sein Kennwort zurücksetzen?

## <a name="policy-enforcement"></a>Durchsetzung von Richtlinien

Während Schäden Steuerelement und Risiken Verringerung-Phasen ist es wichtig, die Effekte tatsächlichen und potenziellen von Angriffen schnell zu verringern. Die Aktion, die Sie können zu diesem Zeitpunkt den Unterschied zwischen einer Nebenversion und eine Haupt-vornehmen. Die genaue Reaktion hängt von Ihrer Organisation und der Art von den Angriffen, den Sie konfrontiert sind. Wenn es sich bei die erste Bewertung geschlossen, dass ein Konto beschädigt wurde, müssen Sie die Richtlinie erzwingen, um dieses Konto zu sperren. Das ist nur ein Beispiel, in dem das Identität Managementsystem genutzt werden wird. Verwenden Sie die folgenden Fragen, mit deren Hilfe Sie Ihre Identität Hybrid-Lösung entwerfen, während Sie unter Berücksichtigung wie Richtlinien erzwungen werden, um auf ein laufendes Vorfall zu reagieren:

- Verfügt Ihr Unternehmen Richtlinien direkte für Benutzer sperren aus Access im Netzwerk bei Bedarf?
 - Wenn Ja, kann die aktuelle Lösung mit dem Hybrid Identität Managementsystem werden integriert, die Sie getroffen abgelegt werden?
- Benötigt Ihr Unternehmen Erzwingen von bedingten Zugriff für Benutzer, die in Quarantäne sind? 
 
>[AZURE.NOTE]
Vergewissern Sie sich zum Erfassen von Notizen für jede Antwort und die Gründe für die Antwort zu verstehen. [Strategie für den Datenschutz definieren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) wird über die verfügbaren Optionen und vor-und Nachteile der einzelnen Optionen wechseln.  Durch Probleme beantwortet diese Fragen, die Sie auswählen, werden die beste Option ist für Ihr Unternehmen geeignet ist.

## <a name="next-steps"></a>Nächste Schritte
[Definieren der Strategie für den Datenschutz](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Siehe auch
[Entwurf Aspekte (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
