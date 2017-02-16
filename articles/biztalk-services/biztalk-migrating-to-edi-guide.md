<properties   
    pageTitle="Migrieren von BizTalk Server EDI-Lösungen zu BizTalk Services technischen Leitfaden | Microsoft Azure"
    description="Migrieren von EDI zu MABS; Microsoft Azure BizTalk-Dienste"
    services="biztalk-services"
    documentationCenter="na"
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags 
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migrieren von BizTalk Server EDI-Lösungen zu BizTalk-Dienste: technischen Leitfaden

Autor: Tim Wieman und Nitin Mehrotra

Bearbeiter: Karthik Bharthy

Mit geschrieben: Microsoft Azure BizTalk Services – Februar 2014 lassen.

## <a name="introduction"></a>Einführung

Elektronische Data Interchange (EDI) ist eine der am häufigsten auftretenden Mittel, nach denen Unternehmen Exchange-Daten elektronisch, auch als Business-to-Business oder B2B Transaktionen bezeichnet. BizTalk Server hatte EDI-Unterstützung für Jahren, seit der ersten BizTalk Server-Veröffentlichung. Mit BizTalk-Diensten weiterhin Microsoft die Unterstützung für EDI-Lösungen auf der Microsoft Azure-Plattform an. B2B Transaktionen sind hauptsächlich außerhalb einer Organisation, und daher ist es einfacher zu implementieren, wenn sie auf eine Cloud implementiert wurde. Microsoft Azure bietet diese Funktion über BizTalk-Dienste.

Während einige Kunden BizTalk-Dienste als "Greenfield" Plattform für die neue EDI-Lösungen betrachten, haben viele Kunden aktuelle BizTalk Server EDI-Lösungen, die Benutzer zur Azure migrieren möchten. Da BizTalk Services EDI ausgelegt ist kann basierend auf die gleiche wichtige Elemente nach BizTalk Server EDI-Architektur (trading Partner, Personen, Agreements), zum Migrieren von BizTalk Server EDI-Elemente zu BizTalk-Dienste.

Dieses Dokument erläutert einige der Unterschiede Migrieren von BizTalk Server EDI-Elemente die BizTalk-Dienste. Dieses Dokument wird davon ausgegangen Kenntnisse BizTalk Server EDI Verarbeitung und Trading Partner Vereinbarungen aus. Weitere Informationen über BizTalk Server EDI finden Sie unter [Trading Partner Management mithilfe von BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welche Version von BizTalk Server EDI-Elementen in BizTalk Services migriert werden kann?

Das BizTalk Server EDI-Modul wurde für BizTalk Server 2010, erheblich verbessert, wenn es neu modelliert wurde Partner, Profile und Vereinbarungen aufnehmen möchten. BizTalk-Dienste verwenden das gleiche Modell zum Organisieren von den Partnern und den Business Abteilungen in diesen Partnern. Daher umfasst die Migration von BizTalk Server 2010 und höher EDI-Elemente zu BizTalk-Dienste, viel mehr direkt weiterleiten. Zum Migrieren von EDI-Elemente, die älter sind als BizTalk Server 2010 zugeordnet müssen Sie zuerst auf BizTalk Server 2010 aktualisieren und migrieren dann Ihre EDI-Elemente zu BizTalk-Dienste.

## <a name="scenariosmessage-flow"></a>Szenarien/Nachrichtenfluss

Als wird EDI in BizTalk Services Verarbeitung mit BizTalk Server, um eine Lösung Trading Partner Management (TPM) erstellt. Die Lösung TPM weist die folgenden Komponenten:

- Trading Partner, darstellen, die Organisation in einer B2B Transaktion.
- Profile, die innerhalb eines Handelspartners Abteilungen darstellen.
- Trading Partner Kaufverträge (und), die im Business-Lizenzvertrag zwischen zwei Partner-Profilen darstellen.

Die folgende Abbildung stellt die gemeinsamkeiten als auch Unterschiede, einen BizTalk Server EDI-Lösung und BizTalk Services EDI-Lösung:

![][EDImessageflow]

Die wichtigsten Unterschiede und gemeinsamkeiten, zwischen einer EDI-Lösung Fluss in BizTalk Server und BizTalk Services sind:

- Wie BizTalk Server ein EDIReceive Verkaufspipeline verwendet eine EDI-Nachricht und eine EDISend Verkaufspipeline zum Senden einer EDI-Nachricht erhalten, verwendet BizTalk-Dienste an einen EDI erhalten Bridge erhalten und einen Controller EDI senden, um EDI-Nachrichten zu senden. BizTalk Server die Rohrleitungen eine Vereinbarung zugeordnet sind, mithilfe von senden oder empfangen Ports. BizTalk-Dienste der Vertrag selbst kennzeichnet senden oder empfangen Bridge.
- Nachdem der EDIReceive Verkaufspipeline die EDI-Nachricht verarbeitet wird die Nachricht in BizTalk Server zu einer SQL Server-Datenbank ausgegeben. Der Verkaufspipeline EdiSend dann nimmt die Nachricht aus der SQL Server-Datenbank, verarbeitet und sendet sie sich an den Handelspartner.

    BizTalk-Dienste klicken Sie nach dem Empfangen der EDI Bridge verarbeitet die EDI-Nachricht, es leitet die Nachricht an einen externen Prozess. Der externe Prozess konnte auf Microsoft Azure oder lokal ausgeführt werden. Der externe Prozess sollte die Nachricht an die EDI senden Brücke weiterleiten; die senden Bridge ziehen grundsätzlich nicht die Nachricht. Nach dem Verarbeiten der Nachricht, leitet die Bridge EDI senden Nachrichten an den Handelspartner.

BizTalk-Dienste bietet eine einfach zu verwendende Konfiguration zum schnellen Erstellen und Bereitstellen eines B2B Vertrags zwischen trading Partner, ohne Konfigurieren eines Microsoft Azure berechnen (Web oder Arbeitskollegen Rollen) Instanzen, alle Microsoft Azure SQL-Datenbanken oder alle Microsoft Azure-Speicher-Konten. Komplexere Szenarien benötigen Verknüpfen von Workflows oder anderen Dienst Verarbeitung "abgeschnittene Ecken" einen Trading Partner-Vertrag, d. h., vor oder nach Trading Partner Vertrag EDI Bridge Verarbeitung. Im Detail auftreten, die folgende Sequenz von Ereignissen während einer EDI-Nachricht Verarbeitung BizTalk-Dienste.

1. Eine EDI-Nachricht empfangen von trading Partner, Fabrikam.  Für den Empfang von EDI-Nachrichten von Partnern, unterstützt BizTalk-Dienste Transportregel-Protokolle, z. B. FTP, SFTP, AS2 und HTTP/S.

2. Die Handel Partner Vertrag erhalten angeordneten Verarbeitung löst die EDI-Nachricht in XML-Format.  Sie können die verschobenen EDI-Nachricht (in XML-Format) an Dienstbus Endpunkte wie einen Endpunkt Service Bus Relay, Service Bus Thema, Service Bus Warteschlange oder eine Verbindung BizTalk-Dienste weiterleiten.

3. Die verschobenen XML-Nachrichten konnte dann aus den Endpunkt für die weitere Verarbeitung von benutzerdefinierten empfangen werden.  Diese Grenzwerte konnte durch eine lokale Komponente oder eine Microsoft Azure berechnen Instanz zu weiteren Verarbeiten der Nachricht in einem Windows-Workflow (WF) oder Windows Communication Foundation (WCF)-Dienst, beispielsweise verarbeitet werden.

4. Die "Verarbeitung angeordneten senden" des Handelspartners Vertrag dann stellt die XML-Nachricht in EDI-Format zusammen und sendet es Handelspartner Contoso.  Zum Senden von Nachrichten EDI an Partnern, unterstützt BizTalk-Dienste die gleichen Protokolle wie die EDI-Nachrichten empfangen.

Dieses Dokument weiteren enthält konzeptionelle Anleitungen migrieren einige der anderen Elemente BizTalk Server EDI aktivieren zu BizTalk-Dienste.

## <a name="sendreceive-ports-to-trading-partners"></a>Empfangen Sie senden/Ports zum Handel Partner

In BizTalk Server richten Sie Orte erhalten und Ports empfangen von Partnern mit EDI/XML-Nachrichten empfangen werden sollen, und senden Ports EDI/XML-Nachrichten an Handelspartner senden einrichten. Sie verschwenden dann diese Ports, um eine separate Partner Vereinbarung treffen mithilfe der BizTalk Server-Verwaltungskonsole. BizTalk-Dienste, die Stelle, an der Sie empfangen von trading Partner Speicherorte und wo Sie senden Nachrichten an trading Partner als Teil der separate Partner Vereinbarung treffen sich selbst (als Teil des Transportregel-Einstellungen) im Portal BizTalk konfiguriert sind.  Daher müssen Sie nicht wirklich des Konzepts der "Sendeports" und "erhalten Orte", pro se, BizTalk-Dienste. Weitere Informationen finden Sie unter [Erstellen von Agreements](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pipelines (Brücken)

In BizTalk Server EDI sind Pipelines Nachricht Verarbeitung von Personen, die auch benutzerdefinierten Logik für die von bestimmter Verarbeitungsfunktionen, je nach Bedarf durch die Anwendung enthalten können. Für BizTalk-Dienste würde die Entsprechung einer EDI Bridge. Jedoch sind BizTalk-Dienste jetzt die EDI Brücken "geschlossen".  Sie können keine Brücke EDI d. h., eigene benutzerdefinierten Aktivitäten hinzufügen. Eine benutzerdefinierte Verarbeitung muss außerhalb der EDI-Brücke in Ihrer Anwendung erfolgen, vor oder nach die Nachricht in der Brücke konfiguriert als Teil der Vereinbarung Trading Partner eingegeben. EAI Brücken haben die Möglichkeit, benutzerdefinierte Verarbeitung führen. Wenn Sie benutzerdefinierte verarbeitet werden sollen, können Sie die EAI Brücken vor oder nach dem die Nachricht von der Brücke EDI verarbeitet wird. Weitere Informationen finden Sie unter [So einbeziehen benutzerdefinierter Code in Brücken](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Sie können einen Fluss Veröffentlichen/Abonnieren mit benutzerdefiniertem Code und/oder mit Warteschlangen und Themen messaging, bevor die separate Partner Vereinbarung treffen die Nachricht empfängt, oder nach der Vertrag verarbeitet die Nachricht und leitet sie an einen Endpunkt Dienstbus Dienstbus einfügen.

Finden Sie unter **Szenarien/Nachrichtenfluss** in diesem Thema für die Nachricht Fluss Muster ein.

## <a name="agreements"></a>Agreements

Wenn Sie mit der BizTalk Server 2010 Trading Partner Agreements für die Verarbeitung von EDI verwendeten vertraut sind, suchen BizTalk-Dienste trading Partner Vereinbarungen sehr vertraut. Die meisten Einstellungen Vertrag identisch sind und die gleichen Begriffe verwendet. In einigen Fällen sind die Einstellungen Vertrag viel einfacher im Vergleich zu den gleichen Einstellungen in BizTalk Server. Microsoft Azure BizTalk Services unterstützt X12, EDIFACT und AS2 zu übermitteln.

Microsoft Azure BizTalk Services bietet auch ein **TPM Datenmigrations** -Tools zum Migrieren von Handel Partner und Vereinbarungen von BizTalk Server Trading Partner Modul BizTalk Services-Portal an. Das TPM Daten Migrationstool steht als Teil eines Pakets Tools, die aus dem [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)heruntergeladen werden können. Das Paket enthält darüber hinaus eine Infos, die Anweisungen und mit dem Tool, grundlegende Informationen zur Problembehandlung für das Tool bietet.

## <a name="schemas"></a>Mithilfe von Schemas

BizTalk-Dienste bietet EDI-Schemas BizTalk Services-Lösungen verwendet werden können.  Darüber hinaus können BizTalk Server EDI-Schemas auch mit BizTalk-Diensten verwendet werden, da der Stammknoten des EDI-Schemas über BizTalk Server als auch BizTalk-Dienste derselben ist. Auf diese Weise werden direkt Optimieren Ihrer BizTalk Server EDI-Schemas und deren Verwendung in den EDI-Lösungen, die Sie entwickeln BizTalk-Dienste verwenden können. Sie können auch die Schemas aus dem [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)herunterladen.

## <a name="maps-transforms"></a>Karten (transformiert)

Karten in BizTalk Server werden können BizTalk-Dienste bezeichnet. Migrieren von Karten von BizTalk Server mit BizTalk Services könnte eine komplexere Aufgaben (je nach Komplexität der Karte) zu erzielen. Das Zuordnungstool für BizTalk-Dienste unterscheidet sich von der BizTalk-Zuordnung. Obwohl der Zuordnung hauptsächlich genauso aussieht, unterscheidet sich das Format der zugrunde liegenden. Die Funktoide (sogenannte **Karte Vorgänge** in BizTalk Services) den Benutzern zur Verfügung unterscheiden sich ebenfalls.  Sie können keine BizTalk-Zuordnung, direkt BizTalk-Dienste verwenden. Darüber werden hinaus nicht alle Funktoide in BizTalk Server als Karte Operationen in BizTalk-Dienste zur Verfügung.

### <a name="new-transform-operations"></a>Neue Datentransformationsvorgänge

Die Liste der verfügbaren Transformation Karte Vorgänge der Zuordnung BizTalk Server völlig anders lösen scheint, müssen BizTalk Services transformiert neue Methoden für die Durchführung der gleichen Aufgaben. Beispielsweise verfügen die BizTalk Services transformiert **Liste Vorgänge** zur Verfügung stehen. Das war nicht in der BizTalk-Zuordnung zur Verfügung.  Die **Liste Vorgänge** können Sie erstellen, und klicken Sie auf "Liste" ausgeführt werden, wo eine Liste eine Reihe von Elementen (auch bekannt als "Zeilen") ist und jedes Element verfügt über mehrere Elemente (auch bekannt als "Spalten") kann.  Sie können die Liste sortieren, wählen Sie die Elemente auf Grundlage einer Bedingung usw..

Ein weiteres Beispiel für die neue Funktionalität in BizTalk Services transformiert sind die **Schleife Vorgänge**an.  Es ist schwierig, geschachtelte Schleifen in der BizTalk Server-Zuordnung zu erstellen.  Auf diese Weise werden die Schleife Karte Vorgänge für die BizTalk Services transformiert hinzugefügt.

Ein weiteres Beispiel ist noch die **Wenn-dann-sonst** Ausdruck Map-Vorgang.  Ausführen eines Wenn-dann-sonst-Vorgangs in der Zuordnung BizTalk möglich war, aber es mehrere Funktoide anscheinend einfacher überflogen erforderlich.

### <a name="migrating-biztalk-server-maps"></a>Migrieren von BizTalk Server zugeordnet ist.

Microsoft Azure BizTalk Services bietet ein Tool zum Migrieren von BizTalk Server können BizTalk-Dienste zugeordnet ist. Die **BTMMigrationTool** steht als Teil der **Tools** -Paket mit den [BizTalk-Dienste SDK herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=235057)bereitgestellt. Weitere Informationen über das Tool finden Sie unter [konvertieren eine BizTalk-Zuordnung zu einer BizTalk Services transformieren](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Sie können auch eine Stichprobe von Sandro Pereira, BizTalk MVP, zum [Migrieren von BizTalk Server Maps BizTalk-Dienste können](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx)eigenständig.

## <a name="orchestrations"></a>Aufweisen, und

Wenn Sie auf Microsoft Azure Verarbeitung BizTalk Server-Orchestrierung migrieren müssen, muss der aufweisen, und neu geschrieben werden, da Microsoft Azure standardisierter BizTalk Server nicht unterstützt.  Die Orchestrierungsfunktionen in einem Windows Workflow Foundation 4.0 (WF4)-Dienst könnten Sie neu schreiben.  Dies wäre komplett überarbeitet, wie es aktuell keine Migration von BizTalk Server aufweisen, und um WF4 ist. Hier sind einige Ressourcen für Windows-Workflow:

- [*Wie ein WCF-Workflow-Dienst mit Bus Servicewarteschlangen und Themen integrieren*](https://msdn.microsoft.com/library/azure/hh709041.aspx) von Paolo Salvatori. 

- [ *Baustein-apps mit Windows Workflow Foundation und Azure* -Sitzung](http://go.microsoft.com/fwlink/p/?LinkId=237314) aus der Konferenz 2011 erstellen.

- [*Windows Workflow Foundation-Entwicklercenter*](http://go.microsoft.com/fwlink/p/?LinkId=237315) auf MSDN.

- [*Windows Workflow Foundation 4 (WF4)-Dokumentation*](https://msdn.microsoft.com/library/dd489441.aspx) auf MSDN.

## <a name="other-considerations"></a>Weitere Überlegungen

Im folgenden werden einige Aspekte, die Sie vornehmen müssen, während Sie BizTalk-Dienste verwenden.

### <a name="fallback-agreements"></a>Alternatives Agreements

Verarbeitung von BizTalk Server EDI weist des Konzepts der "Alternative Vereinbarungen".  BizTalk-Dienste wird **** keine alternative Vertrag Konzept zurückbekommen haben.  Informationen finden Sie BizTalk Dokumentationsthemen [Rolle von Vereinbarungen EDI verarbeiten](http://go.microsoft.com/fwlink/p/?LinkId=237317) und [Konfigurieren globaler oder alternative Vertrag Eigenschaften](https://msdn.microsoft.com/library/bb245981.aspx) auf wie alternative Vereinbarungen in BizTalk Server verwendet werden.

### <a name="routing-to-multiple-destinations"></a>Routing an mehrere Ziele

BizTalk-Dienste Brücken, im aktuellen Zustand unterstützt keine Weiterleitung von Nachrichten an mehrere Ziele mithilfe einer Veröffentlichen-Abonnieren-Modell. Stattdessen könnten Sie Nachrichten aus eine Verbindung BizTalk-Dienste zu einem Thema Dienstbus weiterleiten denen dann mehrere Abonnements erhalten Sie die Nachricht an mehrere Endpunkt können.

## <a name="conclusion"></a>Abschluss

Microsoft Azure BizTalk-Dienste wird am reguläre Meilensteinen, um weitere Features und Funktionen hinzufügen aktualisiert. Bei jeder Aktualisierung suchen wir nach vorne Unterstützung höhere Funktionen zur Erleichterung End-to-End-Lösungen mit den BizTalk-Dienste und andere Technologien Azure erstellen.

## <a name="see-also"></a>Siehe auch

[Entwickeln, Enterprise-Anwendung mit Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
