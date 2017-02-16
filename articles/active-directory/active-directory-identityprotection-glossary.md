<properties
    pageTitle="Azure-Active Directory-Identität Schutz-Glossar | Microsoft Azure"
    description="Azure-Active Directory-Identität Schutz-Glossar"
    services="active-directory"
    keywords="Schutz der Azure-active Directory-Identität, Cloud-app-Suche, Verwalten von Applications, Sicherheit, Risiken, Risiko Ebene, Sicherheitsrisiko, Sicherheitsrichtlinie, Glossar"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Azure-Active Directory-Identität Schutz-Glossar 


### <a name="at-risk-user"></a>Risiko (Benutzer)  
Ein Benutzer mit mindestens ein aktives Risikoereignisse. 

### <a name="atypical-sign-in-location"></a>Atypische Speicherort   
Eine Anmeldung aus einem geographischen Standort, der nicht für den Benutzer, ähnlich wie Benutzer oder den Mandanten typische.

### <a name="azure-ad-identity-protection"></a>Schutz der Azure AD-Identität    
Ein Sicherheitsmodul von Azure Active Directory, die eine konsolidierte Ansicht in Risikoereignisse und mögliche Sicherheitslücken Auswirkungen eines Unternehmens Identitäten bereitstellt.

### <a name="conditional-access"></a>Bedingte access  
Einer Richtlinie für den sicheren Zugriff auf Ressourcen. Bedingte Access Regeln werden in der Azure-Active Directory gespeichert und von Azure AD vor dem Gewähren des Zugriffs auf die Ressource ausgewertet.  Beispiele für Regeln gehören Einschränken des Zugriffs basierend auf Benutzerstandort einstellen Gerät Gesundheit oder Benutzer Authentifizierungsmethode.

### <a name="credentials"></a>Anmeldeinformationen     
Informationen, die berücksichtigt Kennung und Nachweis der Kennzeichnung, mit dem Zugriff auf lokale und Netzwerk-Ressourcen zu erhalten. Beispiele für Anmeldeinformationen sind Benutzernamen und Kennwörter, Zertifikate und Smart-Karten.

### <a name="event"></a>Ereignis   
Ein Datensatz einer Aktivität in Azure Active Directory.

### <a name="false-positive-risk-event"></a>Falsch positiv (Risikoereignis) 
Ein Ereignis Risikostatus manuell festlegen einer Identitätsschutz Benutzer darauf hinzuweisen, dass das Risikoereignis untersucht wurde fälschlicherweise als Risikoereignis gekennzeichnet wurde.

### <a name="identity"></a>Identität    
Eine Person oder Entität, die mittels Authentifizierung, basierend auf Kriterien, wie etwa das Kennwort oder ein Zertifikat überprüft werden muss.

### <a name="identity-risk-event"></a>Identität Risikoereignis     
AAD-Ereignis, das als abweichenden gekennzeichnet wurde, indem Schutz der Identität und darauf hinweisen, dass eine Identität geworden ist.

### <a name="ignored-risk-event"></a>Ignoriert (Risikoereignis)    
Ein Ereignis Risikostatus manuell festlegen einer Identitätsschutz Benutzer, die angibt, dass das Risikoereignis geschlossen ist, ohne eine Behebungsaktion.

### <a name="impossible-travel-from-atypical-locations"></a>Unmöglich Reisen atypische Orten   
Ein Risikoereignis ausgelöst, wenn zwei Zeichen-ins für den gleichen Benutzer erkannt werden, wo Sie mindestens eines der von einem atypische Anmeldung Speicherort ist und der Zeitraum zwischen den Sign-ins kürzer als die minimale Zeit ist, die physisch zwischen diesen Speicherorten zu folgen Servers dauert.  

###<a name="investigation"></a>Untersuchung    
Die Vorgehensweise zum Überprüfen der Aktivitäten, Protokolle und andere relevante Informationen im Zusammenhang mit der ein Risikoereignis selbst entscheiden, ob oder eindämmende Schritte erforderlich, sind verstehen, wenn und wie die Identität wurde gefährdet und zu verstehen, wie die Identität des betroffenen verwendet wurde.

### <a name="leaked-credentials"></a>Verlorene Anmeldeinformationen  

Ein Risikoereignis ausgelöst, wenn die aktuellen Benutzeranmeldeinformationen (Benutzername und Kennwort) im Web dunkel unsere Virenforscher öffentlich gepostet gefunden werden.

### <a name="mitigation"></a>Reduzierung  
Eine Aktion zu beschränken oder die Möglichkeit zu einem betroffenen Identität oder Gerät ausnutzen, ohne die Identität oder das Gerät an einem sicheren Zustand wiederherstellen eines Angreifers zu unterdrücken. Eine Lösung an wird vorherigen Risikoereignisse im Zusammenhang mit der Identität oder das Gerät nicht aufgelöst werden.

### <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung 
Eine Authentifizierungsmethode, die mindestens zwei Authentifizierungsmethoden, die einen Beitrag enthalten können, muss der Benutzer verfügt, einem solchen Zertifikat; etwas bekannt ist der Benutzer, z. B. Benutzernamen, Kennwörter oder komplexe Kennwörter; physische Attributen, wie etwa einen Fingerabdruck; und persönliche Attribute, z. B. eine persönliche Signatur.

### <a name="offline-detection"></a>Offline-Erkennung   
Die Erkennung von Bildschirmdarstellung auftreten und Bewertung des Risikos eines Ereignisses, wie beispielsweise Anmeldung Versuch nach dem Vorfall, für ein Ereignis, das bereits geschehen ist.

### <a name="policy-condition"></a>Richtlinien Bedingung    
Ein Teil einer Sicherheitsrichtlinie, die die Elemente (Gruppen, Benutzer, apps, Geräteplattformen, Gerät Staaten, IP-Bereiche, Clienttypen) definiert die Richtlinie eingeschlossen oder daraus ausgeschlossen.

### <a name="policy-rule"></a>Richtlinienregel für die     
Der Teil einer Sicherheitsrichtlinie, die Umstände beschreibt, die auslösen würde die Richtlinie, und die Aktionen, wenn die Richtlinie ausgelöst wird.

### <a name="prevention"></a>Prevention  
Eine Aktion, die für die Organisation durch den Missbrauch einer Identität oder Gerät Schaden zu vermeiden verdächtigen oder gefährdet kennen. Eine Aktion Prevention bietet keinen Schutz für das Gerät oder die Identität und vorherigen Risikoereignisse nicht beheben.

### <a name="privileged-user"></a>Berechtigungen (Benutzer)
Ein Benutzer, die zum Zeitpunkt der ein Risikoereignis hatten permanente oder temporäre Administratorberechtigungen an einen oder mehrere Ressource in Azure Active Directory, wie etwa ein globaler Administrator, Abrechnung Administrator, Dienstadministrator, Benutzer, und Ihr Kennwort ein Administrator. 

###<a name="real-time"></a>In Echtzeit    
Finden Sie unter Erkennung in Echtzeit ein.

###<a name="real-time-detection"></a>Erkennung in Echtzeit  
Die Erkennung von Bildschirmdarstellung auftreten und Bewertung des Risikos eines Ereignisses, wie beispielsweise Anmeldung Versuch vor dem Ereignis ist zulässig, um den Vorgang fortzusetzen.

### <a name="remediated-risk-event"></a>Diese beseitigt (Risikoereignis)     
Ein Ereignis Status des Risikos automatisch von gibt an, dass das Risikoereignis diese beseitigt wurde mithilfe der Behebungsaktion standard für diese Art von Risikoereignis Schutz Identität festgelegt. Angenommen, wenn das Kennwort zurückgesetzt wird, automatisch diese viele Risikoereignisse, die darauf hinweisen, dass das vorherige Kennwort gefährdet wurde beseitigt werden.

### <a name="remediation"></a>Behebung     
Eine Aktion, die einer Identität oder ein Gerät, das zuvor verdächtigen oder bekannte gefährdet secure. Eine Behebungsaktion stellt die Identität oder das Gerät an einem sicheren Zustand und vorherigen Risikoereignisse im Zusammenhang mit der Identität oder das Gerät aufgelöst.

### <a name="resolved-risk-event"></a>(Risikoereignis) gelöst   
Ein Ereignis Status des Risikos manuell von einem Benutzer Schutz der Identität festlegen, geschlossen, die angibt, dass der Benutzer eine Aktion geeigneten Sicherheitsmaßnahmen außerhalb Schutz der Identität ausgeführt hat und das Risikoereignis berücksichtigt werden sollen.

###<a name="risk-event-status"></a>Status des Risikos-Ereignis    
Eine Eigenschaft eines Ereignisses Risiko, die angibt, ob das Ereignis aktiv ist und ob geschlossen, den Grund für das Schließen.

###<a name="risk-event-type"></a>Ereignistyp Risiko  
Eine Kategorie für das Risikoereignis, den Typ des Anomalie das Ereignis riskante berücksichtigt werden verursachende angibt.

###<a name="risk-level-risk-event"></a>Risiken Ebene (Risikoereignis)  
Angabe (hoch, Mittel oder niedrig) der Schwere der das Risikoereignis Identitätsschutz Benutzern die Aktionen zu priorisieren erschließen sie um das Risiko in ihrer Organisation zu verringern. 

###<a name="risk-level-sign-in"></a>Risiken Ebene (Anmelden) 
Eine Angabe (hoch, Mittel oder niedrig) der Wahrscheinlichkeit, dass für bestimmte Anmeldung jemand versucht, die Identität des Benutzers zu verwenden.

###<a name="risk-level-user-compromise"></a>Risiken Ebene (Benutzer Kompromisse) 
Eine Angabe (hoch, Mittel oder niedrig) der Wahrscheinlichkeit, dass eine Identität geworden ist.

### <a name="risk-level-vulnerability"></a>Risiken Ebene (Sicherheitsrisiko)  
Angabe (hoch, Mittel oder niedrig) der Schwere der das Sicherheitsrisiko Identitätsschutz Benutzern die Aktionen zu priorisieren erschließen sie um das Risiko in ihrer Organisation zu verringern.

### <a name="secure-identity"></a>Secure (Identität)   
Agieren Sie Behebung wie Kennwort ändern oder Erstellen eines neuen Image zum Wiederherstellen einer möglicherweise gefährdeten Identität in eine uneingeschränkte Zustand Computer.

### <a name="security-policy"></a>Sicherheitsrichtlinie
Eine Zusammenstellung von Regeln für die Standardrichtlinie und Bedingung. Eine Richtlinie kann für Personen, wie Benutzer, Gruppen, apps, Geräte, Geräteplattformen, Gerät Staaten, IP-Bereiche und Auth2.0 Clienttypen angewendet werden. Wenn Sie eine Richtlinie aktiviert ist, wird es ausgewertet immer, wenn eine Entität, die im Lieferumfang von der Richtlinie ein Token für eine Ressource ausgestellt wurde.

### <a name="sign-in-v"></a>Melden Sie sich (V) 
Für eine Identität in Azure Active Directory authentifizieren zu können.

### <a name="sign-in-n"></a>Anmelden (n) 
Der Prozess oder die Aktion Authentifizieren einer Identität in Azure Active Directory und das Ereignis, das diesen Vorgang erfasst werden.

###<a name="sign-in-from-anonymous-ip-address"></a>Anmeldung von anonymen IP-Adresse    
Ein Risikoereignis ausgelöst wurde nach einer erfolgreichen Anmeldung von IP-Adresse, die als eine anonyme Proxy-IP-Adresse angegeben wurde.

###<a name="sign-in-from-infected-device"></a>Anmeldung von infizierte Gerät 
Ein Risikoereignis ausgelöst, wenn eine Anmeldung über eine IP-Adresse stammt, die von betroffenen Geräte verwendet werden, welche aktiv möchten Kommunikation mit einem Server Bot bekannt ist.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Anmeldung von IP-Adresse mit verdächtigen Aktivität 
Ein Risikoereignis ausgelöst, nachdem eine erfolgreiche Anmeldung aus einer IP-Adresse mit einer großen Anzahl von fehlgeschlagener Login-Versuche über mehrere Benutzerkonten über einen kurzen Zeitraum.

###<a name="sign-in-from-unfamiliar-location"></a>Anmeldung von unbekannten Speicherort 
Ein Risikoereignis ausgelöst wurde, wenn ein Benutzer erfolgreich von einem neuen Speicherort (IP, Breite/Länge und ASN) signiert.

###<a name="sign-in-risk"></a>Anmeldung Risiko     
Finden Sie unter Risiko Ebene (Anmelden)

###<a name="sign-in-risk-policy"></a>Anmeldung Risiko Richtlinie  
Eine bedingte Zugriffsrichtlinie, die das Risiko für eine bestimmte Anmeldung wertet und Problembehebungen basierend auf vordefinierten Bedingungen und Regeln gilt.

###<a name="user-compromise-risk"></a>Benutzer Kompromisse Risiko     
Finden Sie unter Risiko Ebene (Benutzer Kompromisse)

###<a name="user-risk"></a>Benutzer Risiko    
Finden Sie unter Risiko Ebene (Kompromisse Benutzer).

###<a name="user-risk-policy"></a>Risiken Ablaufrichtlinie für     
Eine bedingte Zugriffsrichtlinie, die die Anmeldung betrachtet und gilt für Problembehebungen basierend auf vordefinierten Bedingungen und Regeln.

###<a name="users-flagged-for-risk"></a>Benutzer, die für das Risiko gekennzeichnet   
Benutzer, die Risikoereignisse verfügen, die aktive oder diese beseitigt werden

###<a name="vulnerability"></a>Sicherheitsrisiko    
Eine Konfiguration oder eine Bedingung in Azure Active Directory, wodurch Verzeichnis Angriffen ausgesetzt wird oder Risiken.


## <a name="see-also"></a>Siehe auch

- [Schutz der Azure-Active Directory-Identität](active-directory-identityprotection.md)
