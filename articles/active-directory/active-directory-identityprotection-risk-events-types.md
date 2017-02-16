<properties
    pageTitle="Arten von Risikoereignisse von Azure Active Directory Identität Protection entdeckt | Microsoft Azure"
    description="In diesem Thema gibt Ihnen einen detaillierten Überblick über die verfügbaren Typen von Risikoereignisse in Azure Active Directory Identitätsschutz"
    services="active-directory"
    keywords="Schutz der Azure-active Directory-Identität, Cloud-app-Suche, Verwalten von Applications, Sicherheit, Risiken, Risiko Ebene, Sicherheitsrisiko, Sicherheitsrichtlinie"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="markvi"/>

#<a name="types-of-risk-events-detected-by-azure-active-directory-identity-protection"></a>Arten von Risikoereignisse von Azure Active Directory Identität Protection erkannt 

In Azure Active Directory Identität Protection Risiko müssen Ereignisse, die:

- als verdächtig gekennzeichnet wurden

- Geben Sie an, dass eine Identität möglicherweise beschädigt wurde. 

In diesem Thema gibt Ihnen einen detaillierten Überblick über die verfügbaren Typen von Risikoereignisse.


## <a name="leaked-credentials"></a>Verlorene Anmeldeinformationen

Verlorene Anmeldeinformationen finden Sie im Web dunkel von Microsoft-Sicherheitsexperten öffentlich veröffentlicht. Diese Anmeldeinformationen befinden sich normalerweise im nur-Text. Diese Anmeldeinformationen Azure AD-verglichen werden, und wenn eine Übereinstimmung vorliegt, werden diese als "Leaked-Anmeldeinformationen" in den Schutz der Identität gemeldet.

Wegen Anmeldeinformationen, die Risikoereignisse als "Hoch" schwere Risikoereignis, klassifiziert werden, da sie eine klare Mitteilung bieten, den Benutzernamen und das Kennwort für einen Angreifer verfügbar sind.

## <a name="impossible-travel-to-atypical-locations"></a>Unmöglich Reise mit atypische Speicherorten

Diese Risiken Ereignistyp identifiziert zwei Zeichen-ins stammen geografischen fernen Speicherorten, in dem Sie mindestens eines der Speicherorte auch für den Benutzer atypische möglicherweise, ältere Verhalten angegeben. Darüber hinaus ist der Zeitraum zwischen den zwei Zeichen-ins kürzer als die Zeit, die es hätte, den Benutzer zu folgen aus der ersten Position der zweiten, gibt an, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet wird. 

Dieser Computer Learning Algorithmus, die offensichtlich "*falsche positive*" zu der Bedingung unmöglich Reisen, z. B. VPN und regelmäßig durch andere Benutzer in der Organisation verwendete Speicherorte beitragen ignoriert.  Das System verfügt über einen Zeitraum initial Learning 14 Tage, während, die sie einen neuen Benutzer anmelden Verhalten lernfähig.

Unmöglich Reisen ist in der Regel ein guter Indikator, den ein Hacker in der Lage, anmelden erfolgreich war. False-Positives können jedoch auftreten, wenn ein Benutzer unterwegs ist, verwenden ein neues Gerät oder verwenden ein VPN, das nicht in der Regel durch andere Benutzer in der Organisation verwendet wird. Eine weitere Quelle für False-positive ist Anwendungen, die nicht ordnungsgemäß Server IP-Adressen als Client IP-Adressen, weiterzuleiten, wodurch das Erscheinungsbild verleihen, möglicherweise Sign-ins stattgefunden hat aus dem Data Center, wo die Anwendung der Back-End-gehostet wird (häufig sind Microsoft Rechenzentren, die das Erscheinungsbild Sign-ins aufzeichnen geben möglicherweise von Microsoft, die im Besitz IP-Adressen platzieren). Als Ergebnis dieser falsch-positive ist die Risiko Ebene für dieses Risikoereignis "**Mittel**".

##<a name="sign-ins-from-infected-devices"></a>Melden Sie sich-ins aus infizierten Geräte

Diese Risiken Ereignistyp identifiziert Sign-ins von Geräten mit Malware infiziert, die bekanntermaßen aktiv mit einem Bot Server kommunizieren. Dies wird durch die IP-Adressen des Benutzers Gerät gegen IP-Adressen, die mit einem Server Bot wurden abgleichen bestimmt. 

Dieses Risikoereignis identifiziert IP-Adressen nicht Benutzer Geräte. Wenn mehrere Geräte hinter einem einzelnen IP-Adresse sind und nur einige sind durch ein Netzwerk Bot, melden Sie sich-ins von anderen Geräten Meine Auslösen dieses Ereignis unnötigerweise, gesteuert, daher zur Klassifizierung von Risikoereignis als "**Niedrig**" wird.  

Es empfiehlt sich, dass Sie wenden Sie sich an den Benutzer und Geräte des Benutzers scannen. Es ist es möglich, dass eines Benutzers persönliche Gerät infiziert ist oder wie zuvor schon erwähnt, wurde dieser Person als Benutzer ein infizierte Gerät aus derselben IP-Adresse verwenden. Infizierte Geräte infiziert sind häufig nach Schadsoftware, die noch nicht durch Antivirensoftware identifiziert wurde, und möglicherweise auch als schlechte Listen, die das Gerät infiziert wird möglicherweise verursacht haben.

Weitere Informationen dazu, wie Sie Adresse Malware infiziert finden Sie unter der [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


## <a name="sign-ins-from-anonymous-ip-addresses"></a>Melden Sie sich-ins von anonymen IP-Adressen

Diese Risiken Ereignistyp identifiziert Benutzer, die über eine IP-Adresse angemeldet haben, die als eine anonyme Proxy-IP-Adresse angegeben wurde. Diese Proxys werden von Personen verwendet, die IP-Adresse des Geräts ausblenden möchten, und für Absichten verwendet werden kann.

Es empfiehlt sich, dass Sie sofort wenden Sie sich an den Benutzer aus, um zu überprüfen, wenn anonyme IP-Adressen verwenden wurden. Die Risiko Ebene für diesen Ereignistyp Risiko ist "**Mittel**" an, da in selbst eine anonyme IP-Adresse nicht von einem Konto Kompromisse ein klarer Hinweis ist.

## <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Melden Sie sich-ins von IP-Adressen mit verdächtigen Aktivität

Diese Risiken Ereignistyp identifiziert IP-Adressen, aus denen eine hohe Anzahl fehlgeschlagener Versuche, über mehrere Benutzerkonten, über einen kurzen Zeitraum angezeigt wurden. Dies entspricht Muster im Datenverkehr von IP-Adressen von Angreifern verwendet und ein sicherer Indikator, dass Konten entweder bereits sind oder Pfeile zum gefährdet wird. Hierbei handelt es sich um einen Computer learning Algorithmus, die offensichtlich "*False-positive*", beispielsweise IP-Adressen, die durch andere Benutzer in der Organisation regelmäßig verwendet werden ignoriert.  Das System verfügt über einen Zeitraum initial Learning 14 Tage, in dem sie das Verhalten Anmeldung eines neuen Benutzers und neuen Mandanten lernfähig.

Wir empfehlen, wenden Sie sich an den Benutzer aus, um zu überprüfen, ob sie tatsächlich über eine IP-Adresse angemeldet ist, die als verdächtig gekennzeichnet wurde. Da verschiedene Geräte hinter die IP-Adresse, möglicherweise, während Sie nur einige der verdächtige Vorgang verantwortlich möglicherweise, dass der Risiko auf diesen Ereignistyp "**Mittel**". 


## <a name="sign-in-from-unfamiliar-locations"></a>Anmeldung von unbekannten Positionen

Diese Risiken Ereignistyp ist ein in Echtzeit Auswertung Verfahren, die ältere Anmeldung Speicherorte betrachtet (IP, Breite / Länge und ASN) um neue / unbekannten Speicherorten zu bestimmen. Das System speichert Informationen zu vorherigen Orte, die von einem Benutzer verwendet, und diese Orte "vertraute" betrachtet. Das Risiko wird auch ausgelöst, wenn es sich bei der Anmeldung von einem Speicherort, die noch nicht in der Liste der Speicherorte vertraut ist, erfolgt. Das System verfügt über einen Zeitraum initial Learning 14 Tage, während die nicht als unbekannten Speicherorte neue Speicherorte kennzeichnen. Das System ignoriert auch Sign-ins aus der vertrauten Geräte sowie Orte, die geografischen nahe einer Position vertraut sind. <br>
Unbekannten Speicherorte können einen klarer Hinweis bereitstellen, den ein Angreifer versuchen, eine gestohlene Identität verwenden kann. False-Positives können auftreten, wenn ein Benutzer unterwegs, und probieren Sie ein neues Gerät oder ein neues VPN verwendet. Als Ergebnis dieser falsch-positive ist die Risiko Ebene für diesen Ereignistyp "**Mittel**".


## <a name="azure-ad-anomalous-activity-reports"></a>Azure AD abweichenden Aktivität-Berichte

Einige dieser Risikoereignisse wurden über die Azure AD abweichenden Berichte Azure-Portal zur Verfügung. Die folgende Tabelle enthält die verschiedenen Arten von Risiken Ereignis und den entsprechenden **Azure AD abweichenden Aktivität** Bericht. Microsoft wird investieren in dieser Leerzeichen und Pläne kontinuierlich verbessern die Genauigkeit der Erkennung von vorhandenen Risiko Ereignissen und Hinzufügen von neuen Risiken Ereignistypen kontinuierlichen Produktlizenzierung fortgesetzt. 



| Ereignistyp Risiko "Schutz" Identität | Entsprechende Azure AD abweichenden Aktivitätsbericht |
| :-- | :-- |
| Verlorene Anmeldeinformationen    | Benutzer mit verlorene Anmeldeinformationen |
| Unmöglich Reise mit atypische Speicherorten | Unregelmäßiges Aktivität |
| Melden Sie sich-ins aus infizierten Geräte    | Melden Sie sich-ins von möglicherweise infizierten Geräte |
| Melden Sie sich-ins von anonymen IP-Adressen  | Melden Sie sich-ins aus unbekannten Quellen |
| Melden Sie sich-ins von IP-Adressen mit verdächtigen Aktivität | Melden Sie sich-ins von IP-Adressen mit verdächtigen Aktivität |
| Vorzeichen in unbekannten Orten    | - |
| Sperrung Ereignisse    | - |

Die folgenden Azure AD abweichenden Berichte nicht als Risikoereignisse in Azure AD-Identität Schutz enthalten sind, und können daher nicht zur Verfügung über den Schutz der Identität. Diese Berichte stehen immer noch Azure-Portal jedoch sie irgendwann in der Zukunft veraltet sein werden, wie sie durch das Risikoereignisse im Schutz der Identität ersetzt werden werden.

- Melden Sie sich-ins nach mehreren Fehlern
- Melden Sie sich-ins aus mehreren geografischen Standorten


## <a name="see-also"></a>Siehe auch

- [Schutz der Azure-Active Directory-Identität](active-directory-identityprotection.md)


