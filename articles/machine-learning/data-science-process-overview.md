<properties
    pageTitle="Was ist Team Daten Wissenschaft Prozess?  | Microsoft Azure"
    description="Team von Daten Science ist eine systematische Methode zum Erstellen von intelligenter Clientanwendungen, die erweiterte Analytics nutzen."
    keywords="Daten Wissenschaft Prozess, Daten Wissenschaft teams"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="what-is-the-team-data-science-process-tdsp"></a>Was ist das Team Daten Wissenschaft Prozess (TDSP)?

Das Team Daten Wissenschaft Prozess (TDSP) bietet einen systematischen Ansatz zum Erstellen von intelligenten Applications, der ermöglicht Teams von Daten Wissenschaftler zu Effektivere Kommunikation über die vollständige Lebenszyklus von Aktivitäten, die zum Aktivieren von diesen Anwendungen in Produkte erforderlich sind.

Insbesondere bietet die TDSP aktuell Daten Wissenschaft Teams mit:

- **Vorgehensweise**: Es werden eine Abfolge von Schritten, die eine Anleitung zum Definieren des Problems, relevante Daten analysieren, erstellen und auswerten Vorhersage Modelle Entwicklungszyklus definieren, und dann diese Modelle in Enterprise-Clientanwendungen bereitstellen.
- **Ressourcen**: Tools und Technologien wie die Daten Wissenschaft virtueller Computer zum Einrichten von Umgebungen für Daten Wissenschaft Aktivitäten und praktischen Anleitung für neue Technologien auf Einführung zu vereinfachen.

So sieht der Entwicklungszyklus des Prozesses Wissenschaft Team Daten aus:

![Diagramm: Daten Wissenschaft Prozess für teams ](./media/data-science-process-overview/data-science-process-for-teams-diagram.png)


Der Vorgang ist **iterative**: das Verständnis der neuen und vorhandenen oder eingrenzen im Modell weiterentwickelt und erforderlich ist, müssen die Schritte in der Sequenz zuvor abgeschlossen. Vorhandene organisationsinterne Entwicklung und Projekt planen Prozesse sind **einfach angepasst** für die Arbeit mit der Schrittfolge TDSP definiert.

Der Prozessschritte werden als Diagramm wiedergegeben und im [TDSP Learning Pfad](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) verknüpft und folgenden beschrieben.  


## <a name="planning-and-preparation-steps"></a>Planung und Vorbereitung Schritte

## <a name="p1-business-and-technology-planning"></a>P1. Business und Technologie Planung

Beginnen Sie ein Projekt Analytics, indem Sie Ihren geschäftlichen und Probleme definieren. Sie werden im Hinblick auf **geschäftliche Anforderungen**angegeben. Ein zentrales Ziel dieses Schritts ist die wichtigsten geschäftlichen Variablen (sales SCHÄTZER oder die Wahrscheinlichkeit eines Reihenfolge gefälschte, beispielsweise) zu identifizieren, die die Analyse Vorhersagen, damit diese Anforderungen erfüllt werden muss. Zusätzliche Planung ist dann in der Regel grundlegende Kenntnisse erforderlich, die Ziele des Projekts aus Sicht der analytischen behoben **Datenquellen** entwickeln möchte. Es ist nicht selten, z. B. feststellen, dass die vorhandenen Systeme erfassen und melden Sie sich zusätzliche Arten von Daten, um das Problem zu beheben und zu erreichen der Projektziele müssen. Leitfaden für die finden Sie unter [Planen Ihrer Umgebung für das Team Daten Wissenschaft Prozess](machine-learning-data-science-plan-your-environment.md) und [Erweiterte Analytics Azure Computer interessante Szenarien](machine-learning-data-science-plan-sample-scenarios.md).  


## <a name="p2-plan-and-prepare-infrastructure"></a>P2. Planen und Vorbereiten der Infrastruktur

Eine Analytics-Umgebung für das Team Daten Wissenschaft Prozess umfasst mehrere Komponenten:

- **Daten Arbeitsbereiche** , in dem die Daten für die Analyse und Modellierung bereitgestellt werden,
- eine **Verarbeitung Infrastruktur** für vorab Verarbeitung, untersuchen und Modellieren von Daten
- eine **Infrastruktur Runtime** Prozessen umsetzen der analytischen Modelle und ausgeführt werden, den intelligente Clientanwendungen nutzen die Modelle.  

Die Analytics-Infrastruktur, die muss eingerichtet werden, ist häufig Teil einer Umgebung, die aus laufenden Systemen Core getrennt ist. Aber sie normalerweise nutzt Daten aus mehreren Systemen innerhalb des Unternehmens und aus Quellen außerhalb des Unternehmens. Die Analytics-Infrastruktur kann rein cloudbasierte sein oder eine lokale Installation oder eine Mischung aus beiden. Optionen finden Sie unter [Einrichten von Daten für Wissenschaft Umgebungen zur Verwendung im Team Daten Wissenschaft Prozess](machine-learning-data-science-environment-setup.md).


## <a name="analytics-steps"></a>Analytics Schritte aus:  

## <a name="1-ingest-the-data-into-the-data-platform"></a>1. die Daten in die Datenplattform Aufnahme

Dieser erste Schritt besteht, um die relevanten Daten aus verschiedenen Quellen, entweder von innerhalb oder von außerhalb des Unternehmens, in einer Umgebung Analytics einzufügen, wo die Daten verarbeitet werden können. Das **Format** der Daten an der Quelle kann der vom Ziel erforderliche Format variieren. Daher müssen möglicherweise einige Datentransformation auch nach der Aufnahme Werkzeuge erfolgen. Optionen finden Sie unter [Laden von Daten in Speicher-Umgebungen für analytics](machine-learning-data-science-ingest-data.md)

Zusätzlich zu den ursprünglichen Aufnahme Daten sind viele intelligente Clientanwendungen erforderlich, um die Daten als Teil eines Prozesses laufenden Learning regelmäßig zu aktualisieren. Dies kann das Einrichten eines **Daten Verkaufspipeline** oder Workflows erfolgen. Hieraus Teil des iterative Teils des Prozesses, die neu erstellt und erneut Auswertung von der intelligente Anwendung bereitstellen der Lösung verwendeten analytical Modelle enthält. Sehen Sie, beispielsweise [Verschieben von Daten aus einer mit lokalen SQLServer in SQL Azure mit Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-visualize-the-data"></a>2. durchsuchen und Visualisieren von Daten

Im nächsten Schritt wird einem besseres Verständnis der Daten durch Untersuchung läuft seine **Zusammenfassende Statistiken**Beziehungen und mithilfe von Techniken solche **Visualisierung**abrufen. Dies ist auch die Stelle, an der der **Qualität der Daten** und die Integrität, wie z. B. fehlende Werte, Daten einen Typenkonflikt und inkonsistente Daten Beziehungen Probleme behandelt werden. Vor der Verarbeitung können werden verwendet, um die unformatierten Daten vor weiteren Analytics zu bereinigen und Modellierung stattfinden kann. Eine Beschreibung finden Sie unter [Erlernen von Aufgaben an, um Daten für erweiterte Rechner vorbereiten](machine-learning-data-science-prepare-data.md).


## <a name="3-generate-and-select-features"></a>3. generieren Sie, und wählen Sie die features

Daten Wissenschaftler, in Zusammenarbeit mit Domänenexperten müssen Identifizieren der Features, die die Vertriebsstrategie Eigenschaften der Datenmenge erfassen und am besten die wichtigsten geschäftlichen Variablen während der Planung identifiziert Vorhersagen verwendet werden können. Diese neuen Features aus bestehenden Daten abgeleitet werden können oder erfordern möglicherweise zusätzliche Daten erfasst werden sollen. Dieses Verfahren wird als **Feature technisch** bezeichnet und ist eine der die wichtigsten Schritte zum Erstellen einer effektiven Vorhersage Analytics-System. Dieser Schritt erfordert eine kreative Kombination von Fachwissen und die Einsichten aus den Daten datenauswertung Schritt abgerufen. Leitfaden für die finden Sie unter [technisch beim Team Wissenschaft Daten bereitstellen](machine-learning-data-science-create-features.md).


## <a name="4-create-and-train-machine-learning-models"></a>4 Erstellen Sie und Schulen Sie maschinellen Learning-Modelle

Daten Wissenschaftler erstellen analytical Modelle zum Vorhersagen der wichtigsten Variablen identifiziert durch die geschäftliche Erfordernisse in der Planung Schritt durchführen, indem die Daten, die bereinigt wurde und Featurized definiert. Maschinelle Learning Systeme unterstützt mehrere **modeling Algorithmen** , die auf einer Vielzahl von Fällen angewendet werden. Leitfaden für die finden Sie unter [So Algorithmen für Azure maschinellen Learning auswählen](machine-learning-algorithm-choice.md).

Daten Wissenschaftler müssen wählen Sie am besten geeignete Modell für die jeweiligen Vorhersage Aufgaben, und es kommt, dass die Ergebnisse aus mehreren Modellen werden, um die besten Ergebnisse zu erhalten kombiniert müssen. Die eingegebenen Daten für die Modellierung ist normalerweise zufällig in drei Teile unterteilt:

- eine Schulung Datasets zurück,
- eine Überprüfung Datengruppe zurück
- eine testen Datengruppe zurück

Die Modelle werden mithilfe der **Datenmenge Schulung**erstellt. Die optimale Kombination von Datenmodellen (mit den Parametern optimiert) ist durch Ausführen der Modelle und Messen der Fehler Vorhersage für die **Überprüfung Datenmenge**ausgewählt. Schließlich wird der **Datenmenge testen** verwendet, um die Leistung des ausgewählten Modells unabhängige Daten auswerten, die nicht zum Schulen oder Validieren des Modells verwendet wurde.  Verfahren finden Sie unter [So Modell Leistung Azure Computer interessante ausgewertet werden soll](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-the-models-in-the-product"></a>5. bereitstellen Sie und nutzen Sie die Modelle in das Produkt

Wenn wir eine Reihe von Datenmodellen, die auch ausführen haben, können sie für andere Programme nutzen **operationalized** werden. Je nach den Anforderungen Business wurden Vorhersagen in **Echtzeit** oder auf Basis **Stapel** vorgenommen. Um operationalized werden, müssen die Modelle mit **Öffnen API-Oberfläche** verfügbar gemacht werden, die einfach aus verschiedenen Programmen genutzt wird solche online-Website, Kalkulationstabellen, Dashboards oder Textzeile Business und Back-End-Applikationen. Finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).


## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

Das [Team Daten Wissenschaft Prozess](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) kann als eine Abfolge von Schritten iterierte verwendet die **bieten einen Leitfaden** für die Vorgänge benötigt, um erweiterte Analyse verwenden, um eine intelligente Anwendung zu erstellen. Jeder Schritt enthält auch Details zur Verwendung von verschiedenen Microsoft-Technologien zum Abschließen der Vorgänge beschrieben.

Während TDSP nicht vorschreiben, bestimmte Arten von **Dokumentation** Elementen, ist es eine bewährte Methode, die Ergebnisse der Durchsuchen von Daten, Modellierung und Auswertung Dokument und den relevanten Code speichern, sodass die Analyse durchlaufen werden kann, wenn erforderlich. Dies ermöglicht außerdem Wiederverwendung von der Analytics Arbeit, bei der Arbeit an andere Programme, die ähnliche Daten und Vorhersagefunktionen Aufgaben betreffen.

Vollständige End-to-End-Vorgehensweisen, in denen alle Schritte im Prozess für **bestimmte Szenarien** veranschaulicht werden ebenfalls bereitgestellt. Sie werden mit den Miniaturansichten Beschreibungen im [Team Daten Wissenschaft Prozess Vorgehensweisen](data-science-process-walkthroughs.md) Thema aufgeführt.
