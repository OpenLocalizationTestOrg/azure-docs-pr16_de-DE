<properties 
    pageTitle="Daten Factory Anwendungsfall-- Produkt Empfehlungen" 
    description="Informationen Sie zu einer Anwendungsfall-mithilfe von Azure Data Factory zusammen mit anderen Diensten implementiert." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Verwenden von Case - Produkt Empfehlungen 

Azure Data Factory ist der viele Dienste verwendet, um die Cortana Intelligence Suite Lösung Zugriffstasten implementieren.  Siehe [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) Seite Details dieser Sammlung. In diesem Dokument beschreiben wir eine allgemeine Anwendungsfall-, die Azure Benutzer haben bereits gelöst und mit Azure Data Factory und andere Cortana Intelligence Komponentendienste implementiert.

## <a name="scenario"></a>Szenario

Im Allgemeinen möchten Online Einzelhändler verleiten ihre Kunden zum Kauf von Produkten präsentieren sie mit Produkten, die sie am ehesten von Interesse sein, und daher wahrscheinlich kaufen wird. Um dies zu erreichen, müssen online Einzelhändler online-Benutzeroberfläche des Benutzers mithilfe der individuellen Produkt Empfehlungen für diesen Benutzer bestimmte anpassen. Diese individuellen Empfehlungen sind, basierend auf deren aktuelle und zurückliegende Einkaufs-Verhaltensdaten, Produktinformationen, neu eingefügten Marken und Produkte und Kunden Segmentierungsdaten vorgenommen werden.  Darüber hinaus können sie die Benutzer Produkt Empfehlungen basierend auf Analyse der allgemeine Verwendung Verhalten von allen ihren Benutzern kombiniert bieten.

Ziel solche Einzelhändler ist für Benutzer auf zum Verkauf Konvertierungen optimieren und höheren Umsatz verdienen.  Diese Konvertierung erzielen, Sie durch die Bereitstellung von kontextbezogene, Verhalten-basierten Produkt Empfehlungen basierend auf Kunden Interessen und Aktionen. Für diese Anwendungsfall-verwenden wir online Einzelhändler als Beispiel für Unternehmen, die für ihre Kunden optimieren möchten. Wenden Sie jedoch diese Prinzipien in jedem Unternehmen, die seiner Kunden um seine waren und Dienstleistungen populärer und Optimieren ihrer Kunden Kauf Erfahrung mit personalisierten Produkt Empfehlungen möchte.

## <a name="challenges"></a>Herausforderung

Es gibt verschiedene Aspekte denen online Einzelhändler konfrontiert, bei dem Versuch, diese Art von Anwendungsfall-implementieren. 

Zunächst unterschiedliche Größen und Shapes Daten aus mehreren Datenquellen, aufgenommen werden müssen beide lokal und in der Cloud. Diese Daten enthält Produktdaten, zurückliegende Verhalten Kundendaten und Benutzerdaten, wie der Benutzer die Website online-Einzelhandel durchsucht. 

Zweite, individuellen Produkt Empfehlungen müssen einigermaßen und genau berechnet und regressionsgleichung werden. Zusätzlich zu Produkt, Marke und Kundendaten Verhalten und Browser müssen online Einzelhändler auch Feedback von Kunden vorhergehenden Käufen zu berücksichtigen bei der Festlegung der das Produkt empfohlenen für den Benutzer einbeziehen. 

Die Empfehlungen muss, sofort für den Benutzer einen nahtlose durchsuchen und Erfahrung erwerben, und Vorschläge, wie die letzten und relevanten Lieferung an. 

Schließlich müssen Einzelhändler messen die Effizienz ihrer Ansatz durch verfolgen generelle Weiterverkauf Cross-verkaufen, klicken Sie auf zur Konvertierung sales-Erfolge und zu deren zukünftigen Empfehlungen anpassen.

## <a name="solution-overview"></a>Übersicht über die Lösung

Dieses Beispiel Anwendungsfall-wurde gelöst und von real Azure Benutzern mit Azure Data Factory und andere Dienste Cortana Intelligence Komponente, einschließlich [HDInsight](https://azure.microsoft.com/services/hdinsight/) und [Power BI](https://powerbi.microsoft.com/)implementiert.

Online Einzelhändler verwendet einen Azure Blob-Speicher, einer lokalen SQLServer, Azure SQL-DB und einer relationalen Datamart als Speicheroptionen für ihre Daten in den Workflow an.  Der Blob-Speicher enthält Kundeninformationen, Verhalten Kundendaten und Produktdaten-Informationen. Die Informationen Produktdaten umfasst Marke Produktinformationen und ein Produkt Katalog gespeicherten lokal in einem SQL Datawarehouse. 

Alle Daten kombiniert und in einem Produkt Empfehlungen System zum Vorführen einer individuelle Empfehlungen auf Kunden Interessen und Aktionen basiert, während der Benutzer Produkte im Katalog auf der Website navigiert eingezogen ist. Die Kunden Siehe auch Produkte, die mit dem Produkt verknüpft sind, deren Eigenschaften, denen Sie ansehen, für insgesamt Website Verwendungsmuster basiert, die für jeden Benutzer nicht verwandt sind.

![Anwendungsfalldiagramme](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabyte unformatierten Web-Protokolldateien werden täglich aus den online Einzelhändler Website als teilweise strukturierten Dateien generiert. Die unformatierten Web-Protokolldateien und die Kunden- und Produktinformationen Kataloginformationen ist aufgenommen regelmäßig in einer Azure Blob-Speicher der Factory-Daten Global bereitgestellten Daten Bewegung als Dienst verwenden. Die unformatierten Protokolldateien für den Tag werden im BLOB-Speicher zur Archivierung (durch Year und Month) aufgeteilt.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) wird verwendet, die unformatierten Protokolldateien im Blob-Speicher und die Protokolle der Motor angesaugten am Skala mit sowohl die Struktur Schwein Skripts zu verarbeiten. Im Web partitionierte protokolliert Daten wird dann zum Extrahieren die benötigten Eingaben für einen Computer learning Empfehlungen System zum Generieren der individuellen Produkt Empfehlungen verarbeitet.

Empfehlungen System verwendet wird, für den Computer learning in diesem Beispiel ist ein learning Empfehlungen-Plattform von [Apache Mahout](http://mahout.apache.org/)open-Source-Computer.  Alle [Azure maschinellen Learning](https://azure.microsoft.com/services/machine-learning/) oder benutzerdefinierten Modell kann mit dem Szenario angewendet werden.  Das Modell Mahout wird die Ähnlichkeit zwischen Elementen auf der Website basierend auf Verwendungsmustern für insgesamt Vorhersagen und generieren, die die Grundlage für einzelnen Benutzers individuellen Empfehlungen verwendet.

Schließlich wird das Resultset empfohlenen individuelle Produkt in einer relationalen Datamart für die Ernährung von der Website Einzelhändler verschoben.  Das Resultset konnte auch direkt aus dem Blob-Speicher von einer anderen Anwendung zugegriffen oder auf zusätzlichen Speicher für andere Nutzer und die Verwendung von Fällen verschoben werden.

## <a name="benefits"></a>Vorteile

Optimieren ihrer Product Empfehlungen Strategie und es mit geschäftlichen Zielen auszurichten, erfüllt die Lösung den online Einzelhändler Merchandising und marketing Ziele. Darüber hinaus konnten sie Prozessen umsetzen und Verwalten von Workflows Empfehlungen Produkt in einer Weise effizienter, zuverlässigen und kostengünstiger. Der Ansatz erleichterte es aktualisieren Sie ihre Modell und seine Effektivität basierend auf der sales auf-zu-Konvertierung Erfolge Maßnahmen optimieren. Mithilfe von Azure Data Factory konnten sie deren Zeit- und manuelle Cloud ressourcenverwaltung aufgeben und wechseln zum ressourcenverwaltung bei Bedarf Cloud. Daher konnten sie einsparen von Zeit und Geld, und deren schnellere Bereitstellung der Lösung. Datenherkunft Datenansichten und Betrieb Dienststatus geworden ist einfach zu visualisieren und Problembehandlung bei mit intuitive Daten Factory Überwachung und Verwaltung Benutzeroberfläche vom Azure-Portal zur Verfügung. Ihre Lösung kann, damit alle Daten zuverlässig gefertigt und für Benutzer bereitgestellt und Daten und Verarbeitung Abhängigkeiten werden automatisch ohne personenbezogenen Eingriff verwaltet jetzt geplant und verwaltet werden.

Durch diese individuellen Einkaufswagen zu verbessern, online erstellte einen Kunden Weitere Mitbewerber, ansprechende Einzelhändler und Funktionalität sales und generelle Kunden daher Akzeptanz.



  