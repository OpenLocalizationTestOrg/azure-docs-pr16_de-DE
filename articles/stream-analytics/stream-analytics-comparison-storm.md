<properties
    pageTitle="Analytics Plattformen: Apache Storm Vergleich mit Stream Analytics | Microsoft Azure"
    description="Erhalten Sie Anleitungen eine Analytics Cloud mithilfe eines Apache Storm Vergleichs zum Stream Analytics auswählen. Grundlegendes zu Funktionen und Unterschiede."
    keywords="Analytics-Plattform, Analytics Plattformen, Cloud Analytics-Plattform Storm Vergleich"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Auswählen einer streaming Analytics-Plattform helfen: Apache Storm Vergleich zu Azure Stream Analytics

Abrufen von Anleitungen eine Analytics Cloud mithilfe dieser Apache Storm Vergleich zu Azure Stream Analytics auswählen. Verstehen Sie, dass die Nutzen der Stream Analytics im Vergleich zu Apache Storm als verwalteter Dienst auf Azure HDInsight, damit Sie die richtige Lösung für Ihr Unternehmen wählen können Fällen verwenden.

Sowohl Analytics Plattformen bieten Vorteile einer Lösung PaaS, es gibt ein paar wichtige hier aufführen-Funktionen, die sie unterschieden werden können. Funktionen landen sowie die Einschränkungen dieser Dienste helfen unten aufgeführten auf die Lösung, die Sie Ihre Ziele erreichen können müssen.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Vergleich mit Stream Analytics Storm: allgemeine Funktionen ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Öffnen der Quelle</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nein, Azure Stream Analytics ist ein Microsoft-proprietär Geschenk.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, ist Apache Storm eine Technologie Apache lizenziert.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft unterstützt</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardwareanforderungen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Es gibt keine hardwareanforderungen aus. Azure Stream Analytics ist ein Azure-Dienst.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Es gibt keine hardwareanforderungen aus. Apache Storm ist ein Azure-Dienst.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Oberste Ebene Einheit</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Mit Azure Stream Analytics Kunden bereitstellen und streaming Aufträge überwachen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Mit Apache Storm auf HDInsight Kunden bereitstellen und überwachen einen ganzen Cluster, der mehrere Storm Aufträge sowie aufgrund der Ergebnisse (einschließlich Stapel) hosten kann.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kurs</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics kostet Lautstärke der verarbeiteten Daten und die Anzahl der streaming Einheiten (pro Stunde, die der Auftrag ausgeführt wird) erforderlich.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Weitere Informationen zu Preisen finden Sie hier.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Für Apache Storm auf HDInsight die Maßeinheit Erwerb basiert auf Cluster und werden in Rechnung gestellt basiert auf der Zeit, die der Cluster, unabhängig von der Einzelvorgänge bereitgestellt ausgeführt wird.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Weitere Informationen zu Preisen finden Sie hier.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Klicken Sie auf jede Analytics-Plattform Authoring##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Funktionen: SQL-DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja, ist eine benutzerfreundliche SQL-Unterstützung für Sprachen verfügbar.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nein, müssen Benutzer Schreiben von Code in Java c# oder Trident-APIs verwenden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Funktionen: Zeitliche Operatoren</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Standardmäßig werden Fenster Aggregate und zeitliche Verknüpfungen unterstützt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Zeitliche Operatoren müssen, vom Benutzer implementiert werden soll.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Erfahrung in der Entwicklung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interaktive authoring und für das Debuggen erzielen über Azure-Portal auf Beispieldaten.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Entwicklung, wird für das Debuggen und Erfahrung für die Überwachung, bereitgestellt über die Visual Studio-Benutzeroberfläche für Benutzer für Java und anderen Sprachen Entwickler .NET IDE ihrer Wahl möglicherweise verwenden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Unterstützt das Debuggen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics bietet grundlegende Aufgabe Status- und Vorgänge als ein Verfahren für das Debuggen, aber derzeit bietet keine Flexibilität in was/wie viele die Protokolle ie zu bieten hat: ausführlichen Modus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Detaillierte Protokolle sind für das Debuggen verfügbar. Es gibt zwei Methoden zum Einbinden Protokolle für Benutzer, können Sie über visual Studio oder Benutzer RDP in der Cluster Protokolle zugreifen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Unterstützung für UDFs (User Defined Funktionen)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Es wird derzeit nicht unterstützt für UDFs.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDFs können in c#, Java oder die Sprache Ihrer Wahl geschrieben werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Extensible - benutzerdefiniertem code</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Es gibt keine Unterstützung für extensible Code in Stream Analytics aus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, es gibt Verfügbarkeit zum Schreiben von benutzerdefinierten Codes in c#, Java oder anderen unterstützten Sprachen auf Storm.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Datenquellen und Ausgaben##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Eingabe von Datenquellen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Die unterstützten Eingabewerte Quellen sind Azure Ereignis Hubs und Azure-Blobs.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Es gibt Verbinder für Ereignis Hubs, Dienstbus, Kafka verfügbar. Nicht unterstützte Verbinder möglicherweise über benutzerdefiniertem Code implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Eingabedaten Formate</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Unterstützte Eingabeformate sind Avro, JSON und CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Alle Format möglicherweise über benutzerdefiniertem Code implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ausgaben</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Möglicherweise müssen Sie ein streaming Auftrag mehrere Ausgaben. Unterstützte Ausgaben: Azure Ereignis Hubs, Azure Blob-Speicher, Azure-Tabellen, SQL Azure-DB und PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Unterstützung für viele Ausgaben in einer Suchtopologie, kann jede Ausgabe benutzerdefinierten Logik für die Verarbeitung von untergeordneten verfügen. Im Paket enthält Storm Connectors für PowerBI, Azure Ereignis Hubs, Azure Blob-Speicher, Azure DocumentDB, SQL und HBase an. Nicht unterstützte Verbinder möglicherweise über benutzerdefiniertem Code implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Codierung Datenformaten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics erfordert UTF-8 Datenformat genutzt werden.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Alle Codierung Datenformat möglicherweise über benutzerdefiniertem Code implementiert werden.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Verwaltung und Betrieb##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Modell zur Bereitstellung der Position</strong>
                </p>
                <p>
                    - <strong>Azure-Portal</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Bereitstellung wird über Azure-Portal, PowerShell und REST-APIs implementiert.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment wird über Azure-Portal, PowerShell, Visual Studio und REST-APIs implementiert.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Überwachung (Stats, Indikatoren usw.).</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Überwachung wird über Azure-Portal und dem restlichen APIs implementiert.
                </p>
                <p>
Der Benutzer kann auch Azure Benachrichtigungen konfigurieren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Überwachung wird über Storm Benutzeroberfläche und REST-APIs implementiert.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skalierbarkeit</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Anzahl der Einheiten Streaming für jedes Projekt. Jede Einheit Streaming verarbeitet bis zu 1 MB/s. Max standardmäßig 50 Einheiten. Anruf zu erhöhen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Anzahl der Knoten im Cluster HDI Storm. Keine Beschränkung auf die Anzahl der Knoten (obere Grenze durch Ihre Azure Kontingent definiert). Anruf zu erhöhen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Grenzwerte für Datenverarbeitung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Benutzer können skalieren, nach oben oder unten Anzahl Streaming Einheiten Datenverarbeitung vergrößern oder Kosten optimieren.
                </p>
                <p>
Bis zu 1 GB/s skalieren </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer kann nach oben oder unten Clustergröße Anforderungen skalieren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Beenden, fortsetzen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Beenden und fortsetzen aus dem letzten Ort beendet.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Anhalten und Fortsetzen der letzten platzieren beendet basierend auf das Wasserzeichen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Update-Dienst und framework</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatische Patch ohne Ausfallzeiten.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatische Patch ohne Ausfallzeiten.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Geschäftskontinuität über einen hochgradig Dienst mit garantiert SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Vereinbarung zum SERVICELEVEL von 99,9 % Verfügbarkeit </p>
                <p>
Automatische Wiederherstellung von Fehlern </p>
                <p>
Wiederherstellung dynamische zeitliche Operatoren ist integriert.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Vereinbarung zum SERVICELEVEL von 99,9 % Verfügbarkeit von Storm Cluster. Apache Storm ist eine gegeben streaming Fehlerstrukturanalyse-Plattform, jedoch der Kunden dafür verantwortlich ist, um sicherzustellen, dass ihre streaming Arbeit ohne Unterbrechung ausgeführt werden.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Erweiterte Funktionen##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Verspätete Ankunfts- und Behandlung von außerhalb der Reihenfolge von Ereignissen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Integrierte zu konfigurierender Richtlinien, um die Reihenfolge zu ändern, legen Sie Ereignisse, oder passen die Uhrzeit des Ereignisses.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer muss Logik zur Behandlung dieses Szenario implementieren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bezug Daten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Verweisdaten aus Azure Blobs mit maximale Größe von 100 MB Nachschlagen in-Memory-Cache zur Verfügung. Aktualisieren von Daten Bezug wird vom Dienst verwaltet werden.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Keine Einschränkungen auf Datengröße. Der Verbinder ist für HBase, DocumentDB, SQL Server und Azure verfügbar. Nicht unterstützte Verbinder möglicherweise über benutzerdefiniertem Code implementiert werden.
                </p>
                <p>
Aktualisieren von Daten Bezug muss von benutzerdefiniertem Code behandelt werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integration in Computer-Schulung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Als Funktionen veröffentlicht während ASA Auftrag Erstellung <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(als "Privat" Preview)</a>Azure maschinellen Learning-Modelle, durch konfigurieren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Verfügbar bis Storm Schrauben.
                </p>
            </td>
        </tr>
    </tbody>
</table>
