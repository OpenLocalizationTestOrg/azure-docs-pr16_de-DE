<properties
   pageTitle="Sicherheitscenter Preise | Microsoft Azure"
   description="Dieser Artikel enthält Informationen zu Preise für Azure-Sicherheitscenter."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/12/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-pricing"></a>Azure-Sicherheitscenter Preise

Azure-Sicherheitscenter hilft Ihnen der verhindern, erkennen und Beantworten von Risiken mit größerer Einblick in und Kontrolle über die Sicherheit Ihrer Azure Ressourcen. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

## <a name="pricing-tiers"></a>Preise Ebenen

Das Sicherheitscenter ist in zwei Versionen angeboten:

- Die **kostenlose Ebene** wird auf alle Azure-Abonnements automatisch aktiviert. Die kostenlose Ebene bietet einen Einblick in den Sicherheitszustand von Azure Ressourcen, grundlegende Sicherheitsrichtlinie, Mittelpunkt und Integration in Security-Produkte und Dienste von Partnern.
- Der **Standard-Ebene** fügt hochentwickelter Erkennung-Funktionen, einschließlich Bedrohungsanalyse, Verhaltensanalyse, Normalbetriebswerte, Sicherheitsvorfälle und Bedrohung Bewertungsberichte hinzu. Eine **kostenlose Testversion von 90 Tagen** steht für die Standard-Leiste.

Weitere Informationen finden Sie unter [Seite Preise](https://azure.microsoft.com/pricing/details/security-center/)Sicherheitscenter.

> [AZURE.NOTE] Sicherheitscenter verwendet Azure-Speicher zum Speichern von Sicherheitsdaten aus Ihrem geschützten Knoten generiert. Kosten im Zusammenhang mit dieser Speicher fallen nicht unter den Preis für den Dienst und unterliegen separat zu regulären [Sätzen Azure-Speicher](https://azure.microsoft.com/pricing/details/storage/blobs/). Speicher anfallen auch während der Testversion.

## <a name="try-standard-free-for-90-days"></a>Versuchen Sie es Standard 90 Tage lang kostenlos

Eine kostenlose Testversion 90 Tage steht für die Standard-Leiste. Wenn Sie die kostenlose Testversion der Standard-Stufe erhalten möchten, wählen Sie die **Richtlinie** Kachel auf das **Sicherheitscenter** Blade aus. Wählen Sie das Abonnement, das Sie auf Standard aktualisieren möchten. Wählen Sie auf der Blade **Sicherheitsrichtlinie** **Preise Ebene**aus. Wählen Sie auf das **Auswählen der Preisgestaltung Ebene** Blade **Standard – kostenlose Testversion**ein.

![Kostenlose Testversion][1]

Am Ende von 90 Tagen wird Sie auswählen sollten, um den Vorgang fortzusetzen, den Dienst verwenden, automatisch zunächst für Verwendung belasten.

## <a name="upgrade-to-standard"></a>Upgrade auf Standard

Aktualisieren Sie auf der Standard-Ebene Erweiterte Erkennung hinzufügen. Um die Standardansicht Ebene erhalten, wählen Sie die **Richtlinie** Kachel auf das **Sicherheitscenter** Blade aus. Wählen Sie das Abonnement, das Sie auf Standard aktualisieren möchten. Wählen Sie auf der Blade **Sicherheitsrichtlinie** **Preise Ebene**aus. Wählen Sie **Standard**aus, klicken Sie auf das Blade **Auswählen der Preisgestaltung Ebene** .

![Standard-Ebene][2]

## <a name="why-upgrade-to-standard"></a>Warum ein upgrade auf Standard?

Die standardmäßige Ebene der Sicherheitscenter enthält alle Features des kostenlosen Ebene sowie erweiterte Erkennung. Erweiterte Erkennung hilft beim Identifizieren des aktiver Risiken Azure Ressourcen verwendet und bietet Ihnen die Einsichten schnell reagiert erforderlich.

Sicherheitscenter beschäftigt erweiterte Sicherheit Analytics, die Signatur-basierten Ansätze weit hinausgehen. Durchbrüche in big Data und Computer learning Technologien werden genutzt, um auszuwerten Ereignisse, über den gesamten Cloud-Struktur – erkennen von Angriffen, die zum Identifizieren der manuellen Ansätze verwenden und die Entwicklung von Angriffen Vorhersage möglich ist.

Sicherheit Analytics, die im Zusammenhang mit der standardmäßigen Ebene sind:

- **Bedrohungsanalyse** – sucht nach bekannten Bad Akteuren mithilfe der globalen Bedrohungsanalyse aus Microsoft-Produkten und Diensten, die Microsoft digitale Crimes Einheit, Microsoft Security Response Center und externen feeds
- **Verhalten Analysis** - gilt für bekannte Muster zum Ermitteln von bösartigen Verhaltens
- **Normalbetriebswerte** - verwendet statistische Profil erstellen, um einen zurückliegenden Basisplan zu erstellen. Benachrichtigt Sie abweichungen von definierte Basislinien, die eine mögliche Angriffen entsprechen

In der nachstehenden **von Sicherheitshinweisen** Blade hat Sicherheitscenter eines Wertpapiers **Vorfall**festgestellt. Ein Sicherheitsvorfall ist eine Aggregation alle Benachrichtigungen für eine Ressource, die mit Kill Kette Mustern ausrichten. Auswählen des Sicherheitsvorfalls werden weitere Details zu den Vorfall und die zugehörigen Benachrichtigungen Listen. Auswählen einer Benachrichtigung enthält weitere Informationen zu diesen vorkommen.

![Sicherheitsvorfall][3]

Die folgenden **Netzwerk-Kommunikation** Benachrichtigung enthält Details zur Warnung. Details zu zählen, deren vollständige Beschreibung, deren schwere, aktuellen Zustand (die in diesem Fall geschlossen wird d. h., der Benutzer ausgeführt hat Aktion zu schließen,), die betroffenen Ressourcen und Schritte zur Wiederherstellung. Es gibt auch eine Liste mit Links zu Microsoft-Bedrohungsanalyse Berichten. Diese Berichte können für Sicherheitsmaßnahmen und defensive Zwecke verwendet werden.

![Sicherheit benachrichtigen details][4]

## <a name="enable-data-collection"></a>Aktivieren der Sammlung von Daten

Klicken Sie zum Aktivieren von virtuellen Computern Verhalten Analytics muss Datensammlung eingeschaltet sein. Sie müssen möglicherweise Datensammlung aktivieren, oder wenn Sie erstmals Sicherheitscenter zugreifen, wenn Sie eine Sicherheitsrichtlinie erstellen.

Um zu überprüfen, dass Datensammlung aktiviert ist, wählen Sie die **Richtlinie** -Kachel aus. Das Blade **Sicherheitsrichtlinie** wird geöffnet, wobei Azure-Abonnements. Wählen Sie ein Abonnement aus. Wenn **Datensammlung** deaktiviert ist, ändern Sie ihn auf, und speichern Sie die Änderung. Finden Sie unter [Datensammlung in Azure Sicherheitscenter aktivieren](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Nächste Schritte

- In diesem Dokument wurden Sie eingeführt werden die Gebühren für das Sicherheitscenter. Zusätzliche Preisinformationen finden Sie unter [Seite Preise](https://azure.microsoft.com/pricing/details/security-center/)Sicherheitscenter.
- Weitere Informationen zu den Sicherheitscenter Erkennung erweiterte Funktionen finden Sie unter [Sicherheitscenter Azure Erkennungsfunktionen](security-center-detection-capabilities.md).
- Wenn Sie Fragen zur Nutzung von Sicherheitscenter haben, finden Sie unter den [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md).
- Wenn Sie noch Fragen zur Nutzung von Sicherheitscenter oder Azure haben, besuchen Sie die [Foren Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/free-trial.png
[2]: ./media/security-center-pricing/standard.png
[3]: ./media/security-center-pricing/incident.png
[4]: ./media/security-center-pricing/network-alert.png
