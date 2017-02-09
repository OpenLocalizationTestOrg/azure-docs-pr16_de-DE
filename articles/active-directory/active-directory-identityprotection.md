<properties
    pageTitle="Azure-Active Directory-Schutz | Microsoft Azure"
    description="Erfahren Sie, wie Azure AD-Schutz können Sie die Möglichkeit eines Angreifers zu einem betroffenen Identität oder Gerät ausnutzen und eine Identität oder ein Gerät, das vorher vermutet oder bekannte gefährdet wurde gesichert zu beschränken."
    services="active-directory"
    keywords="Schutz der Azure-active Directory-Identität, Cloud-app-Suche, Verwalten von Applications, Sicherheit, Risiken, Risiko Ebene, Sicherheitsrisiko, Sicherheitsrichtlinie"
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
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Schutz der Azure-Active Directory-Identität 

Azure Active Directory-Schutz ist ein Feature der P2 Azure AD Premium Edition, die Sie mit einer konsolidierten Ansicht in Risikoereignisse und mögliche Sicherheitslücken Auswirkungen Ihrer Organisation Identitäten bereitstellt. Microsoft Cloud-basierte Identitäten für Jahren Schutz wurde, und mit Azure AD-Identität Schutz Microsoft ist bereitstellen dieselben Schutz Systeme für Enterprise-Kunden. Schutz der Identität nutzt vorhandene Azure AD-Anomalie Erkennungsfunktionen (verfügbar über Azure AD-abweichenden Aktivitätsberichte) und stellt neue Risiko Ereignistypen, für die Bildschirmdarstellung auftreten in Echtzeit erkennen können.



##<a name="getting-started"></a>Erste Schritte

Die meisten Sicherheitsverstöße stattfinden, wenn Angreifern Zugriff auf einer Umgebung, die durch Diebstahl der Identität des Benutzers erhalten. Angreifer zunehmend effektiver bei Nutzung von Drittanbieter-Verletzung, und verwenden anspruchsvolle Phishing-Angriffen geworden. Nachdem eine er Zugriff auf sogar ein niedrig berechtigten Benutzerkonto verschafft hat, ist es für den Zugriff auf wichtige Unternehmensressourcen durch seitliche Bewegung einfach. Deshalb ist es wichtig, alle Identitäten schützen und, wenn eine Identität beschädigt ist, die vorausschauende verhindern, dass die betroffenen Identität missbraucht werden. 

Entdecken betroffenen Identitäten ist keine einfache Aufgabe. Glücklicherweise Identitätsschutz helfen können: Schutz der Identität verwendet adaptive maschinellen Learning Algorithmen und Heuristik Bildschirmdarstellung auftreten erkennen und Risiko Ereignisse, die hindeuten, dass eine Identität geworden ist.
 
Mit diesen Daten generiert Identitätsschutz Berichte und Benachrichtigungen, die ermöglicht es Ihnen, diese Risikoereignisse ermitteln, und führen Sie die geeigneten Sicherheitsmaßnahmen oder Minderungsaktion.
 
Aber Azure Active Directory Identität Protection ist mehr als ein Tool Überwachung und Berichterstattung. Basierend auf Risikoereignisse, berechnet Schutz der Identität eine Risiko Benutzerebene für jeden Benutzer, aktivieren Sie das Risiko basierende Richtlinien, um die Identitäten Ihrer Organisation automatisch schützen konfigurieren.  Diese Risiken basierende Richtlinien, zusätzlich zu den anderen bedingte Access-Steuerelementen von Azure Active Directory und EMS, bereitgestellten können automatisch blockieren oder anbieten adaptive Behebungsaktionen, die das Zurücksetzen von Kennwörtern und kombinierte Authentifizierung Durchsetzung enthalten.  

####<a name="explore-identity-protections-capabilities"></a>Schutz der Identität des Funktionen durchsuchen 

**Erkennen von Risikoereignisse und riskant Konten aus:**  

- Erkennen von 6 Risiko Ereignistypen Computer lernen und heuristische Regeln verwenden 

- Berechnen der Benutzer Risiko Ebenen

- Bereitstellen von benutzerdefinierten Empfehlungen Gesamt-Sicherheitslage zu verbessern, indem Sie die Hervorhebung Schwachstellen



**Untersuchen von Risiken Ereignissen:**

- Senden von Benachrichtigungen für Risikoereignisse

- Untersuchen von Risiken Ereignissen mit relevanten und kontextbezogene Informationen

- Bereitstellen von einfachen Workflows zum Nachverfolgen von Untersuchungen

- Den einfachen Zugriff auf Behebungsaktionen, wie das Zurücksetzen von Kennwörtern



**Bedingte Access Risiko basierende Richtlinien:**

- Riskante Sign-ins zu verringern, indem Sie Sign-ins blockieren oder kombinierte Authentifizierung Probleme mit Anforderung der Richtlinie.

- Richtlinie blockieren oder secure riskante Benutzerkonten

- Richtlinie Benutzer registrieren, für die kombinierte Authentifizierung erforderlich ist.


## <a name="detection-and-risk"></a>Erkennung und Risiken

### <a name="risk-events"></a>Risikoereignisse

Risikoereignisse sind Ereignisse, die durch Schutz der Identität als verdächtig gekennzeichnet wurden, und darauf hinzuweisen, dass eine Identität möglicherweise beschädigt wurde. Eine vollständige Liste der Risikoereignisse finden Sie unter [Typen von Risikoereignisse von Azure Active Directory Identität Protection erkannt](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Risiken Ebene

Die Risiko Ebene für ein Risikoereignis ist eine Angabe der Schwere der das Risikoereignis (hoch, Mittel oder niedrig) an. Die Risiko Ebene hilft Identitätsschutz Benutzer priorisieren Sie die Aktionen, die sie ausführen müssen, um das Risiko in ihrer Organisation zu verringern. Die Schwere des Ereignisses Risiko stellt die Stärke der das Signal als eine Vorhersage der Identität Kompromisse, kombiniert mit der Menge an Rauschen, die sie in der Regel führt. 

- **Hohe**: hohes vertrauen und hoher Priorität Risikoereignis. Diese Ereignisse sind sicherer Indikatoren, die die Identität des Benutzers gefährdet, und alle Benutzerkonten beeinträchtigt sofort diese beseitigt werden.

- **Mittel**: hoher Priorität, aber niedriger KONFIDENZ Risikoereignis, oder umgekehrt. Diese Ereignisse sind potenziell riskante und alle Benutzerkonten beeinträchtigt diese beseitigt sein sollte.

- **Niedrig**: niedriger KONFIDENZ und niedrig schwere Risikoereignis. Dieses Ereignis möglicherweise nicht sofortige eine Aktion erforderlich, aber in Kombination mit anderen Risikoereignisse vorsehen einen klarer Hinweis, dass die Identität beschädigt wurde. 


![Risiken Ebene] (./media/active-directory-identityprotection/01.png "Risiken Ebene")

 

Risikoereignisse sind entweder identifiziert **Echtzeit**, oder platzieren Sie in der neuen Verarbeitung aus, nachdem das Risikoereignis bereits genommen hat (offline). Derzeit die meisten Risikoereignisse in Schutz der Identität offline berechnet werden, und innerhalb von 2 bis 4 Stunden in Schutz der Identität angezeigt. Während der ausgewerteten in Echtzeit, die Risikoereignisse in Echtzeit werden angezeigt in der Identität Schutzkonsole innerhalb von 5 bis 10 Minuten.

Mehrere legacy-Clients unterstützen derzeit nicht in Echtzeit Risiko Ereignis Erkennung und Prevention. Daher Sign-ins von diesen Clients nicht erkannt oder in Echtzeit verhindert.


## <a name="investigation"></a>Untersuchung
Ihre Reise durch Identitätsschutz beginnt in der Regel mit dem Dashboard Schutz der Identität. 

![Behebung] (./media/active-directory-identityprotection/1000.png "Behebung")

Das Dashboard bietet Ihnen den Zugriff auf:
 
- Berichte, wie **Benutzer für das Risiko gekennzeichnet**, **Ereignisse Risiken** und **Schwachstellen**
- Einstellungen wie die Konfiguration von **Richtlinien für Sicherheit**, **Benachrichtigungen** und **kombinierte Authentifizierung Registrierung**
 

Es ist in der Regel Ihr Ausgangspunkt für Untersuchung, also die Vorgehensweise zum Überprüfen der Aktivitäten, Protokolle und andere relevante Informationen im Zusammenhang mit der ein Risikoereignis selbst entscheiden, ob oder eindämmende Schritte erforderlich sind, und wie die Identität wurde gefährdet und zu verstehen, wie die Identität des betroffenen verwendet wurde.

Sie können Ihre Untersuchung Aktivitäten, die [Benachrichtigungen](active-directory-identityprotection-notifications.md) aneinander anpassen, die Azure Active Directory-Schutz pro e-Mail sendet.

Die folgenden Abschnitte enthalten Sie mit mehr Details und die Schritte, die mit einer Untersuchung verknüpft sind.  



## <a name="what-is-a-user-risk-level"></a>Was ist ein Risiko Benutzerebene?

Eine Ebene der Benutzer Risiko ist eine Angabe der Wahrscheinlichkeit, dass die Identität des Benutzers geworden ist (hoch, Mittel oder niedrig). Es wird basierend auf den Benutzer Risikoereignisse berechnet, die die Identität des Benutzers zugeordnet sind. 

Der Status eines Ereignisses Risiko ist **aktiv** oder **geschlossen**. Nur Risikoereignisse aktiv sein **sollen** , die eigene Notizen hinzufügen können die Benutzer Risiko Berechnung. 

Risiken Benutzerebene rechnet mithilfe der folgenden Eingaben:

- Aktives Risikoereignisse beeinträchtigen des Benutzers
- Risiken Ebene dieser Ereignisse 
- Gibt an, ob alle Behebungsaktionen getroffen haben 

![Benutzer Risiken] (./media/active-directory-identityprotection/1001.png "Benutzer Risiken")



Benutzer Risiko Ebenen verwenden, um bedingte Richtlinien zum riskante Benutzer anmelden blockieren zu erstellen können, oder erzwingen sie sicher ihr Kennwort ändern. 


## <a name="closing-risk-events-manually"></a>Schließen das Risikoereignisse manuell

In den meisten Fällen gelangen Sie Behebungsaktionen wie ein sicheres Kennwort zurücksetzen, um das Risikoereignisse automatisch zu schließen. Jedoch möglicherweise diese nicht immer möglich.  
Dies ist beispielsweise der Fall, wenn:

- Ein Benutzer mit aktives Risikoereignisse wurde gelöscht
- Untersuchungen werden, dass ein Risikoereignis gemeldeten weist seriösen benutzerseitig durchführen wurde

Da das Risikoereignisse, **die aktiv** sind, an der Berechnung des Benutzers Risiko beitragen, müssen Sie möglicherweise eine Risiko Ebene manuell zu verringern, indem Sie das Risikoereignisse manuell zu schließen.  
Im Verlauf der Untersuchung können Sie auswählen, wird eine der folgenden Aktionen aus, um den Status eines Ereignisses Risiko ändern:

![Aktionen] (./media/active-directory-identityprotection/34.png "Aktionen")

- **Beheben** – Wenn Sie der Meinung sind, dass das Risikoereignis berücksichtigt werden sollen und nach Untersuchung läuft ein Risikoereignis, eine Aktion geeigneten Sicherheitsmaßnahmen außerhalb Identitätsschutz beim ursprünglichen markieren geschlossen, Sie das Ereignis als gelöst. Ereignisse werden das Risikoereignis Status auf geschlossen festlegen und das Risikoereignis wird nicht mehr eigene Notizen hinzufügen können Benutzer Risiko aufgelöst wird.

- **Kennzeichnen als falsch-positiv** - In einigen Fällen können Sie ein Risikoereignis ermitteln und feststellen, dass es als eine riskante falsch gekennzeichnet wurde. Sie können die Anzahl der solche Vorkommen zu reduzieren, indem Sie das Risikoereignis als falsch-positiv. Dadurch wird den Computer learning Algorithmen, um die Klassifizierung von ähnliche Ereignisse in der Zukunft zu verbessern. Der Status des falsch-positiv-Ereignisse in **geschlossen** ist und sie werden nicht mehr eigene Notizen hinzufügen können Benutzer Risiko.

- **Ignorieren** – Wenn Sie keine Aktionen Behebung getroffen haben, aber das Risikoereignis aus der Liste der aktiven entfernt werden soll, markieren Sie ein Risikoereignis ignorieren und der Status des Ereignisses wird geschlossen. Ignorierte Ereignisse liefern keine Risiken für Benutzer. Diese Option sollte nur unter Ausnahmefällen verwendet werden. 

- **Reaktivieren** - Risikoereignisse, die manuell geschlossen wurden (indem Sie auf **lösen**, **falsch positiv**oder **ignorieren**) kann erneut aktiviert werden Festlegen des Ereignisses Status auf **aktiv**. Erneut aktivierte Risikoereignisse eigene Notizen hinzufügen können die Benutzer Risiko Ebene Berechnung. Risikoereignisse geschlossen bis Behebung (z. B. ein sicheres Kennwort zurücksetzen) können nicht erneut aktiviert werden. 




Geben Sie **im Konfigurationsdialogfeld verwandte öffnen**:

1. Klicken Sie auf das Blade **Azure AD-Schutz** unter **untersuchen**, auf **Risikoereignisse**.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1002.png "Zurücksetzen von Kennwörtern")

2. Klicken Sie in der Liste **Risikoereignisse** auf ein Risiko.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1003.png "Zurücksetzen von Kennwörtern")

2. Klicken Sie auf das Risiko Blade mit der rechten Maustaste eines Benutzers.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1004.png "Zurücksetzen von Kennwörtern")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Schließen alle Risikoereignisse für einen Benutzer manuell

Statt manuell schließen einzeln Risikoereignisse für einen Benutzer, bietet Schutz der Azure Active Directory Identität auch eine Methode, um alle Ereignisse für einen Benutzer mit einem Klick zu schließen.


![Aktionen] (./media/active-directory-identityprotection/2222.png "Aktionen")

Wenn Sie **alle Ereignisse schließen**klicken, werden alle Ereignisse geschlossen und der betroffene Benutzer ist nicht mehr Risiko.



## <a name="remediating-user-risk-events"></a>Beseitigen Benutzer Risikoereignisse

Eine Behebung handelt es sich um eine Aktion gesichert eine Identität oder ein Gerät, das vorher vermutet oder bekannte gefährdet wurde. Eine Behebungsaktion stellt die Identität oder das Gerät an einem sicheren Zustand und vorherigen Risikoereignisse im Zusammenhang mit der Identität oder das Gerät aufgelöst.

Zur Behebung von Benutzer Risikoereignisse, können Sie folgende Aktionen ausführen:

- Führen Sie ein sicheres Kennwort zurücksetzen, um Benutzer Risikoereignisse manuell zu beheben. 

- Konfigurieren einer Sicherheitsrichtlinie für Benutzer Risiko zum Verringern oder Benutzer Risikoereignisse automatisch beheben

- Abbildung des erneut das infizierte Gerät  


### <a name="manual-secure-password-reset"></a>Manuelle sicheres Kennwort zurücksetzen

Ein sicheres Kennwort zurücksetzen ist eine effektive Behebung für viele Risikoereignisse, bei und ausgeführt werden, automatisch schließt diese Risikoereignisse Risiko Benutzerebene berechnet. Das Dashboard-Schutz können einleiten ein Kennworts für einen Benutzer riskante zurücksetzen. 

Das zugehörige Dialogfeld bietet zwei verschiedene Methoden zum Zurücksetzen eines Kennworts:

**Kennwort zurücksetzen** – Select **Kennwort zurücksetzen Benutzer anfordern** dem Benutzer auf Self wiederherstellen, wenn den Benutzer dürfen wurde für die kombinierte Authentifizierung registriert. Der Benutzer werden während des Benutzers nächsten Anmeldung erforderlich, um eine kombinierte Authentifizierung Herausforderung erfolgreich lösen und klicken Sie dann das Kennwort geändert wird. Diese Option steht nicht zur Verfügung, wenn das Benutzerkonto nicht bereits registriert kombinierte Authentifizierung ist.

**Temporäres Kennwort** : Select **generieren ein temporäres Kennwort** sofort ungültig das vorhandene Kennwort ein, und erstellen ein neues temporäres Kennwort für den Benutzer. Senden Sie eine alternative e-Mail-Adresse für den Benutzer oder seinem Vorgesetzten neue temporäre Kennwort. Da das Kennwort temporär ist, wird der Benutzer zum Ändern des Kennworts bei der Anmeldung aufgefordert werden.


![Richtlinie] (./media/active-directory-identityprotection/1005.png "Richtlinie")


Geben Sie **im Konfigurationsdialogfeld verwandte öffnen**:

1. Klicken Sie auf das Blade **Azure AD-Schutz** auf **Benutzer, die für das Risiko gekennzeichnet**.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1006.png "Zurücksetzen von Kennwörtern")


2. Wählen Sie aus der Liste der Benutzer einen Benutzer mit mindestens ein Risikoereignisse aus.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1007.png "Zurücksetzen von Kennwörtern")


2. Klicken Sie auf das Blade Benutzer auf **Kennwort zurücksetzen**.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1008.png "Zurücksetzen von Kennwörtern")





## <a name="user-risk-security-policy"></a>Benutzer Risiko Sicherheitsrichtlinie

Eine Benutzer Risiko Sicherheitsrichtlinie ist eine bedingte Zugriffsrichtlinie, die die Ebene Risiko auf einen bestimmten Benutzer wertet und Behebung und Reduzierung Aktionen basierend auf vordefinierten Bedingungen und Regeln angewendet.


![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1009.png "Benutzer Ridk Richtlinie")


Azure AD-Schutz können Sie die Reduzierung and Behebung von Benutzern für das Risiko gekennzeichnet, indem es Ihnen ermöglicht zu verwalten:

- Legen Sie die Benutzer und Gruppen, die, denen die Richtlinie gilt: 

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1010.png "Benutzer Ridk Richtlinie")


- Festlegen des Benutzer Risiko Ebene Schwellenwerts (niedrig, Mittel oder hoch), der die Richtlinie auslöst: 

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1011.png "Benutzer Ridk Richtlinie")


- Legen Sie die Steuerelemente erzwungen werden, wenn die Richtlinie auslöst:

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1012.png "Ridk Ablaufrichtlinie für")


- Wechseln Sie den Status Ihrer Richtlinie ein:

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/403.png "MFA Registrierung")


- Überprüfen Sie und Werten Sie den Einfluss der Änderung vor dem Aktivieren aus:

    ![Ridk Ablaufrichtlinie für] (./media/active-directory-identityprotection/1013.png "Benutzer Ridk Richtlinie")


Auswählen eines **hohen** Schwellenwerts reduziert an, wie oft, die eine Richtlinie wird ausgelöst, und den Einfluss auf die Benutzer minimiert.
Es schließt jedoch **Niedrig** , **Mittel** und Benutzer für das Risiko von der Richtlinie, die nicht geschützt werden kann, Identitäten oder Geräte, die zuvor vermutet oder bekannte gefährdet gekennzeichnet.

Wenn die Richtlinie festlegen

- Ausschließen von Benutzern, die wahrscheinlich viele False-positive (Entwickler, Sicherheitsanalysten) generieren

- Ausschließen von Benutzern in Gebietsschemas, in dem das Aktivieren dieser Richtlinie kein praktische (beispielsweise kein Zugriff an den Helpdesk)

- Verwenden ein **hohen** Schwellenwert während der anfänglichen Richtlinie einsatzbereit ab, oder wenn Sie Probleme, die von Endbenutzern gesehen minimieren müssen.

- Verwenden Sie einen **Low** -Schwellenwert aus, wenn Ihre Organisation mehr Sicherheit erfordert. Auswählen eines **Niedrig** Schwellenwerts werden zusätzliche Benutzer Anmeldung Probleme, sondern erhöhte Sicherheit vorgestellt.

Der empfohlene Standardwert für die meisten Organisationen ist so konfigurieren Sie eine Regel für einen Schwellenwert **Mittel** , eine Balance zwischen Nutzbarkeit und Sicherheit zu finden.

Eine Übersicht über die zugehörigen Benutzerfunktionalität finden Sie unter:

- [Compromised Konto Wiederherstellung Fluss](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Compromised Konto blockierter Verkehr](active-directory-identityprotection-flows.md#compromised-account-blocked).  


Geben Sie **im Konfigurationsdialogfeld verwandte öffnen**:

1. Klicken Sie auf vorher **Azure AD-Schutz** , in Abschnitt **Konfigurieren** **Risiko Ablaufrichtlinie für**ein.

    ![Ridk Ablaufrichtlinie für] (./media/active-directory-identityprotection/1009.png "Benutzer Ridk Richtlinie")






## <a name="mitigating-user-risk-events"></a>Verringerung der Benutzer Risikoereignisse
Administratoren können für Benutzer bei der Anmeldung je nach der Ebene Risiko Sicherheit Ablaufrichtlinie für ein Risiko festlegen. 

Blockieren eines anmelden:
 
- Verhindert die Erstellung der neuen Benutzer Risikoereignisse für die betroffenen Benutzer

- Ermöglicht es Administratoren, manuell Behebung von Risikoereignisse beeinträchtigen die Identität des Benutzers und an einem sicheren Zustand wiederherstellen



## <a name="what-is-a-sign-in-risk-level"></a>Was ist eine Anmeldung Risiko Ebene?

Eine Anmeldung Risiko Ebene ist eine Angabe der Wahrscheinlichkeit, dass für bestimmte Anmeldung Person Authentifizierung versucht, mit die Identität des Benutzers (hoch, Mittel oder niedrig). Die Anmeldung Risiko Ebene zum Zeitpunkt der eine Anmeldung ausgewertet und Risikoereignisse betrachtet und Indikatoren erkannt Echtzeit für die bestimmte Anmeldung. 

## <a name="mitigating-sign-in-risk-events"></a>Verringerung der Anmeldung Risikoereignisse 
Eine Lösung ist eine Aktion aus, die ein Angreifer eine beschädigte Identität oder Gerät ausnutzen, ohne die Identität oder das Gerät an einem sicheren Zustand wiederherstellen einschränken. Eine Lösung an wird vorherigen Anmeldung Risikoereignisse im Zusammenhang mit der Identität oder das Gerät nicht aufgelöst werden.

Bedingte Access können in Azure AD-Schutz Sie die um automatisch Anmeldung Risikoereignisse zu verringern. Verwenden die folgenden Richtlinien, erwägen Sie die Ebene Risiko der Benutzer oder der Anmeldung riskante Sign-ins blockieren oder den Benutzer auffordern kombinierte Authentifizierung ausführen. Diese Aktionen können verhindern, dass einen Angreifer ausnutzen einer gestohlen Identität um Schaden zu verursachen, und bieten Ihnen möglicherweise einige Zeit, um die Identität zu schützen. 


## <a name="sign-in-risk-security-policy"></a>Anmeldung Risiko Sicherheitsrichtlinie

Eine Anmeldung Risiko-Richtlinie ist eine bedingte Zugriffsrichtlinie, die das Risiko für eine bestimmte Anmeldung wertet und Problembehebungen basierend auf vordefinierten Bedingungen und Regeln gilt.

![Anmeldung Risiko Richtlinie] (./media/active-directory-identityprotection/1014.png "Anmeldung Risiko Richtlinie")


Azure können AD-Identität Sie die Verminderung von riskante Sign-ins verwalten, indem es Ihnen ermöglicht:

- Legen Sie die Benutzer und Gruppen, die, denen die Richtlinie gilt: 

    ![Anmeldung Risiko Richtlinie] (./media/active-directory-identityprotection/1015.png "Anmeldung Risiko Richtlinie")


- Festlegen des Anmeldung Risiko Ebene Schwellenwerts (niedrig, Mittel oder hoch), der die Richtlinie auslöst: 

    ![Anmeldung Risiko Richtlinie] (./media/active-directory-identityprotection/1016.png "Anmeldung Risiko Richtlinie")


- Legen Sie die Steuerelemente erzwungen werden, wenn die Richtlinie auslöst:  

    ![Anmeldung Risiko Richtlinie] (./media/active-directory-identityprotection/1017.png "Anmeldung Risiko Richtlinie")
    

- Wechseln Sie den Status Ihrer Richtlinie ein:

    ![MFA Registrierung] (./media/active-directory-identityprotection/403.png "MFA Registrierung")

- Überprüfen Sie und Werten Sie den Einfluss der Änderung vor dem Aktivieren aus: 

    ![Anmeldung Risiko Richtlinie] (./media/active-directory-identityprotection/1018.png "Anmeldung Risiko Richtlinie")


### <a name="what-you-need-to-know"></a>Was Sie sonst noch wissen

Sie können eine Anmeldung Risiko Sicherheitsrichtlinie um mehrstufige Authentifizierung erforderlich konfigurieren:

![Anmeldung Risiko Richtlinie] (./media/active-directory-identityprotection/1017.png "Anmeldung Risiko Richtlinie")

Aus Gründen der Sicherheit kann diese Einstellung jedoch nur für Benutzer, die bereits für kombinierte Authentifizierung erfasst wurden. Wenn die Bedingung zu mehrstufige Authentifizierung erforderlich für einen Benutzer erfüllt ist, die noch nicht für kombinierte Authentifizierung registriert ist, wird der Benutzer blockiert. 

Als bewährte Methode Wenn für riskante Sign-ins, mehrstufige Authentifizierung erforderlich sein soll sollten Sie Folgendes:

1. Aktivieren Sie die [kombinierte Authentifizierung Registrierung Richtlinie](#multi-factor-authentication-registration-policy) für die betroffenen Benutzer.
2. Die betroffenen Benutzer für die Anmeldung in einer nicht riskant Sitzung zum Ausführen einer MFA Registrierung erforderlich

Diese Schritte wird sichergestellt, dass die kombinierte Authentifizierung für eine riskante Anmeldung erforderlich ist. 


### <a name="best-practices"></a>Bewährte Methoden

 
Auswählen eines **hohen** Schwellenwerts reduziert an, wie oft, die eine Richtlinie wird ausgelöst, und den Einfluss auf die Benutzer minimiert.  
 
Es schließt jedoch **Niedrig** , **Mittel** und melden Sie sich-ins für das Risiko von der Richtlinie, die wodurch nicht Angreifer aus eine betroffenen Identität ausnutzen blockiert möglicherweise gekennzeichnet. 

Wenn die Richtlinie festlegen 

- Ausschließen von Benutzern, die keine / mehrstufige Authentifizierung nicht möglich

- Ausschließen von Benutzern in Gebietsschemas, in dem das Aktivieren dieser Richtlinie kein praktische (beispielsweise kein Zugriff an den Helpdesk)

- Ausschließen von Benutzern, die wahrscheinlich viele False-positive (Entwickler, Sicherheitsanalysten) generieren

- Verwenden ein **hohen** Schwellenwert während der anfänglichen Richtlinie einsatzbereit ab, oder wenn Sie Probleme, die von Endbenutzern gesehen minimieren müssen.

- Verwenden Sie einen **Low** -Schwellenwert aus, wenn Ihre Organisation mehr Sicherheit erfordert. Auswählen eines **Niedrig** Schwellenwerts werden zusätzliche Benutzer Anmeldung Probleme, sondern erhöhte Sicherheit vorgestellt.

Der empfohlene Standardwert für die meisten Organisationen ist so konfigurieren Sie eine Regel für einen Schwellenwert **Mittel** , eine Balance zwischen Nutzbarkeit und Sicherheit zu finden.

 
Die Anmeldung Risiko Richtlinie ist:

- Alle Browser Datenverkehr und mit modernen Authentifizierung anmelden-ins angewendet.
- Applikationen mit älteren Sicherheitsprotokolle durch den WS-Trust-Endpunkt an den partnerverbundkontakte IDP, wie z. B. ADFS deaktivieren nicht angewendet.

Die Seite **Risikoereignisse** in der Verwaltungskonsole Identitätsschutz Listet alle Ereignisse an:

- Dieser Richtlinie angewendet wurde
- Sie können die Aktivität überprüfen und bestimmen, ob die Aktion entsprechenden wurde 

Eine Übersicht über die zugehörigen Benutzerfunktionalität finden Sie unter:

- [Riskante Wiederherstellung](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Riskante Anmeldung blockiert](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Mehrstufige Authentifizierung Registrierung während riskante anmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





Geben Sie **im Konfigurationsdialogfeld verwandte öffnen**:

1. Klicken Sie auf **Anmeldung Risiko Richtlinie** **Azure AD-Schutz** vorher, in der Sie im Abschnitt **Konfigurieren** .

    ![Ridk Ablaufrichtlinie für] (./media/active-directory-identityprotection/1014.png "Benutzer Ridk Richtlinie")





## <a name="multi-factor-authentication-registration-policy"></a>Mehrstufige Authentifizierung Registrierung Richtlinie

Azure kombinierte Authentifizierung ist eine Methode Ihre Identität zu überprüfen, die die Verwendung von mehr als nur einen Benutzernamen und ein Kennwort erforderlich ist. Es stellt eine zweite Sicherheitsebene Benutzer anmelden-ins und Transaktionen.  
Es empfiehlt sich, dass Sie Azure kombinierte Authentifizierung für Benutzer anmelden-ins da benötigen sie:

- Stellt die strenge Authentifizierung für eine Reihe von Überprüfungsoptionen für einfache

- Spielt eine zentrale Rolle in Ihrer Organisation zum Schützen und Wiederherstellen von Konto Kompromisse vorbereiten

![Ridk Ablaufrichtlinie für] (./media/active-directory-identityprotection/1019.png "Benutzer Ridk Richtlinie")



Weitere Informationen hierzu finden Sie unter [Neuigkeiten Azure kombinierte Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md)


Azure AD-Schutz hilft Ihnen das Verwalten der Verteilen der Registrierung kombinierte Authentifizierung durch Konfigurieren einer Richtlinie, die Sie können: 



- Legen Sie die Benutzer und Gruppen, die, denen die Richtlinie gilt: 

    ![MFA Richtlinie] (./media/active-directory-identityprotection/1020.png "MFA Richtlinie")



- Legen Sie die Steuerelemente erzwungen werden, wenn die Richtlinie auslöst::  

    ![MFA Richtlinie] (./media/active-directory-identityprotection/1021.png "MFA Richtlinie")


- Wechseln Sie den Status Ihrer Richtlinie ein:

    ![MFA Richtlinie] (./media/active-directory-identityprotection/403.png "MFA Richtlinie")

- Zeigen Sie den aktuellen Registrierungsstatus an: 

    ![MFA Richtlinie] (./media/active-directory-identityprotection/1022.png "MFA Richtlinie")


Eine Übersicht über die zugehörigen Benutzerfunktionalität finden Sie unter:

- [Mehrstufige Authentifizierung Registrierung Fluss](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Mehrstufige Authentifizierung Registrierung während riskante anmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





Geben Sie **im Konfigurationsdialogfeld verwandte öffnen**:

1. Klicken Sie auf vorher **Azure AD-Schutz** , in klicken Sie im Abschnitt **Konfigurieren** auf **Registrierung kombinierte Authentifizierung**.

    ![MFA Richtlinie] (./media/active-directory-identityprotection/1019.png "MFA Richtlinie")





## <a name="next-steps"></a>Nächste Schritte

 - [Channel 9: Azure AD- und Identität anzeigen: Identität Schutz Vorschau](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Arten von Risikoereignisse von Azure Active Directory Identität Protection erkannt](active-directory-identityprotection-risk-events-types.md)
 - [Schwachstellen von Azure Active Directory Identität Protection erkannt](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory-Schutz Benachrichtigungen](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory-Schutz playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory-Schutz-Glossar](active-directory-identityprotection-glossary.md)

 - [Melden Sie sich in Erfahrung mit Azure AD-Schutz](active-directory-identityprotection-flows.md)

 - [Aktivieren der Azure-Active Directory-Schutz](active-directory-identityprotection-enable.md)
 - [Azure Active Directory Identität Protection - Benutzer entsperren](active-directory-identityprotection-unblock-howto.md)

 - [Erste Schritte mit Azure Active Directory Identitätsschutz und Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


