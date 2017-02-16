<properties
   pageTitle="Lernprogramm: Verarbeiten EDIFACT Rechnungen mit Azure BizTalk Services | Microsoft Azure BizTalk-Dienste"
   description="So erstellen und Konfigurieren der app Feld Verbinder oder API und diese in einer app Logik in Azure-App-Dienst verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Lernprogramm: Prozess EDIFACT Rechnungen mit Azure BizTalk Services
Sie können das BizTalk-Portal konfigurieren und Bereitstellen von X12 und EDIFACT Agreements. In diesem Lernprogramm betrachten wir zum Erstellen einer EDIFACT Vereinbarung für den Austausch von Rechnungen zwischen Partnern mit ein. In diesem Lernprogramm wird geschrieben, um eine End-to-End-Business-Lösung, die im Zusammenhang mit zwei Partnern, Northwind und Contoso, die EDIFACT Nachrichten austauschen.  

## <a name="sample-based-on-this-tutorial"></a>In diesem Lernprogramm Grundlage Stichprobe
In diesem Lernprogramm wird geschrieben versehen einer Stichprobe, **Senden EDIFACT Rechnungen Using BizTalk Services**, das aus der [MSDN Code Gallery](http://go.microsoft.com/fwlink/?LinkId=401005)Herunterladen zur Verfügung steht. Verwenden Sie das Beispiel, und Navigieren in diesem Lernprogramm zu verstehen, wie im Beispiel erstellt wurde. Oder Sie könnten in diesem Lernprogramm verwenden, um eigene Lösung Grund auf erstellen. In diesem Lernprogramm wird als Ziel in Richtung der zweiten Vorgehensweise verwendet, damit Sie verstehen, wie diese Lösung erstellt wurde. Darüber hinaus so weit wie möglich, das Lernprogramm konsistent mit der Stichprobe und die gleichen Namen für Elemente (z. B. Schemas, Sie können) verwendet, wie Sie in der Stichprobe verwendet.  

>[AZURE.NOTE] Da diese Lösung beinhaltet das Senden einer Nachricht von einer EAI Bridge auf eine Brücke EDI, verwendet es wieder der Stichprobe [BizTalk Services Bridge verketten Stichprobe](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) .  

## <a name="what-does-the-solution-do"></a>Welche Funktion hat die Lösung?

In dieser Lösung erhält Northwind EDIFACT Rechnungen von Contoso an. Diese Rechnungen sind nicht in einem standardmäßigen EDI-Format. Ja, muss vor dem Senden der Rechnung auf Nordwind, es zu einem EDIFACT Rechnung (auch INVOIC genannt) Dokument umgewandelt werden. Bestätigung muss Northwind EDIFACT Rechnung zu verarbeiten, und zurück eine Meldung (auch als CONTRL bezeichnet) zu "Contoso".

![][1]  

Um dieses Szenario Business zu erreichen, verwendet Contoso die Funktionen von Microsoft Azure BizTalk-Dienste.

*   Contoso verwendet EAI Brücken, um die ursprüngliche Rechnung EDIFACT INVOIC transformieren.

*   Die Brücke EAI sendet die Nachricht klicken Sie dann auf eine EDI senden Brücke bereitgestellt, die als Teil einer Vereinbarung im Portal BizTalk konfiguriert.

*   Die EDI senden die EDIFACT INVOIC verarbeitet und leitet sie an Northwind.

*   Nach dem Empfangen der Rechnung, erhalten Northwind gibt eine CONTRL Nachricht dem EDI-Bridge als Teil der Vereinbarung bereitgestellt.  

> [AZURE.NOTE] Klicken Sie optional veranschaulicht diese Lösung auch Batchverarbeitung verwenden, um die Rechnungen stapelweise, statt jede Rechnung separat senden zu senden.  

Wenn Sie das Szenario abgeschlossen haben, verwenden wir Servicebuswarteschlangen auf Rechnung von Contoso Northwind senden oder Empfangen von Bestätigung von Northwind aus. Diese Warteschlangen mithilfe einer Clientanwendung, die zum Download zur Verfügung und ist im Beispiel Verpacken, die zur Verfügung enthalten erstellt werden können als Teil dieses Lernprogramms.  

## <a name="prerequisites"></a>Erforderliche Komponenten

*   Sie müssen einen Namespace Dienstbus. Erstellen Sie einen Namespace Anweisungen finden Sie unter ['s gemacht: Erstellen oder Ändern einer Service Bus Dienst Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Lassen Sie uns setzen voraus, dass bereits einen Dienstbus Namespace nach der Bereitstellung, **Edifactbts**bezeichnet.

*   Sie müssen ein Abonnement BizTalk-Dienste. Anweisungen finden Sie unter [Erstellen eines BizTalk-Dienstes mithilfe von Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=302280). In diesem Lernprogramm lassen Sie uns Angenommen Sie, Sie ein BizTalk-Dienste-Abonnement, **Contosowabs**bezeichnet.

*   Registrieren Sie Ihr Abonnement BizTalk-Dienste, klicken Sie auf das BizTalk-Portal an. Anweisungen finden Sie unter [Registrieren einer BizTalk-Service-Bereitstellung auf das BizTalk-Portal](https://msdn.microsoft.com/library/hh689837.aspx)

*   Sie müssen Visual Studio installiert ist.

*   Sie müssen BizTalk-Dienste SDK installiert haben. Sie können das SDK von [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057) herunterladen.  

## <a name="step-1-create-the-service-bus-queues"></a>Schritt 1: Erstellen der Servicebuswarteschlangen  
Diese Lösung verwendet Servicebuswarteschlangen zum Austauschen von Nachrichten zwischen Partnern. Contoso und Northwind Nachrichten in den Warteschlangen aus, in dem Sie die EAI und/oder EDI Brücken nutzen. Für diese Lösung benötigen Sie drei Servicebuswarteschlangen aus:

*   **Northwindreceive** – erhält Northwind Rechnung von Contoso über diese Warteschlange.

*   **Contosoreceive** – empfängt Contoso die Bestätigung von Northwind über diese Warteschlange.

*   **unterbrochen** – unterbrochen alle Nachrichten an diese Warteschlange weitergeleitet werden. Nachrichten werden angehalten, wenn sie während der Verarbeitung ein Fehler auftreten.

Sie können diese Servicebuswarteschlangen erstellen, mithilfe einer Clientanwendung im Beispielpaket enthalten.  

1.  Öffnen Sie die Position, an der Sie das Beispiel heruntergeladen haben, **Lernprogramm senden Rechnungen verwenden BizTalk Services EDI-Bridges.sln**.

2.  Drücken Sie **F5** , um zu erstellen, und starten Sie die **Lernprogramm** -Clientanwendung aus.

3.  Geben Sie im Bildschirm Service Bus ACS Namespace, Herausgeber Namen und Herausgeber-Taste.

    ![][2]  
4.  Ein Meldungsfeld informiert werden, dass Sie drei Warteschlangen in Ihren Dienstbus Namespace erstellt werden. Klicken Sie auf **OK**.

5.  Lassen Sie den Lernprogramm-Client ausgeführt. Öffnen der, klicken Sie auf **Dienstbus** > **_Ihren Dienstbus Namespace_** > **Warteschlangen**, und stellen Sie sicher, dass die drei Warteschlangen erstellt werden.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Schritt 2: Erstellen Sie und Bereitstellen Sie Handel Partner-Vertrag
Erstellen Sie eine separate Partner Vereinbarung treffen zwischen Contoso und Northwind ein. Eine separate Partner Vereinbarung treffen definiert Trade Vertrag zwischen zwei Business-Partner, wie etwa die Nachrichtenschema zu verwenden, welche messaging-Protokoll verwenden, usw. an. Eine separate Partner Vereinbarung treffen enthält zwei EDI Brücken, eine zum Senden von Nachrichten an Partnern (die **EDI senden Bridge**genannt) und eine zum Empfangen von Nachrichten von Partnern (die **EDI erhalten Bridge**genannt).

Im Kontext dieser Lösung die EDI senden Brücke entspricht der senden-Seite der Vereinbarung und wird verwendet, um die Rechnung EDIFACT von Contoso zu Northwind senden. Auf ähnliche Weise, die EDI empfangen Brücke entspricht der empfangen-Seite der Vereinbarung und wird verwendet, um Empfangsbestätigungen empfangen von Northwind.  

### <a name="create-the-trading-partners"></a>Erstellen der Partnern

Erstellen von Partnern als Einstieg für Contoso und Northwind.  

1.  Klicken Sie im BizTalk-Portal, klicken Sie auf der Registerkarte **Partner** auf **Hinzufügen**.

2.  Klicken Sie in der neuen Partnerseite Geben Sie **Contoso** als Partnernamen, und klicken Sie dann auf **Speichern**.

3.  Wiederholen Sie die Schritte zum Erstellen des zweiten Partners, **Northwind**aus.  

### <a name="create-the-agreement"></a>Erstellen Sie den Vertrag
Handel Partner Agreements werden zwischen Business Profilen trading Partner erstellt. Diese Lösung verwendet die standardmäßige Partnerprofile, die automatisch erstellt werden, wenn wir die Partner erstellt haben.  

1.  Klicken Sie im Portal BizTalk Services auf **Vereinbarungen** > **Hinzufügen**.

2.  Geben Sie in die neue Vereinbarung **Allgemeine** Einstellungsseite die Werte aus, wie in der folgenden Abbildung dargestellt, und klicken Sie dann auf **Weiter**.

    ![][3]  

    Nachdem Sie auf **Weiter**klicken, werden die beiden Registerkarten, **Einstellungen für empfangen** und **Senden** verfügbar.

3.  Erstellen Sie den senden-Vertrag zwischen Contoso und Northwind ein. Dieses Vertrags steuert, wie "Contoso" EDIFACT Rechnung an Northwind sendet.

    1.  Klicken Sie auf **Einstellungen zu senden**.

    2.  Behalten Sie die Standardwerte auf den Registerkarten **Eingehende URL**, **Transformieren**und **Batching** .

    3.  Laden Sie das Schema **EFACT_D93A_INVOIC.xsd** auf der Registerkarte **Protokoll** unter dem Abschnitt **Schemas** hoch. Dieses Schema ist mit dem Beispielpaket verfügbar.

        ![][4]  
    4.  Geben Sie auf der Registerkarte **Transport** die Details für die Servicebuswarteschlangen aus. Für die Vereinbarung senden angeordneten verwenden wir die **Northwindreceive** EDIFACT Rechnung an Northwind senden, die **unterbrochen** Warteschlange und zum Weiterleiten von Nachrichten, die während der Verarbeitung fehl und unterbrochen werden. Sie erstellt diese Warteschlangen in **Schritt 1: erstellen die Servicebuswarteschlangen** (in diesem Thema).

        ![][5]  

        Klicken Sie unter **Transportregel-Einstellungen > transportieren Typ** und **Nachricht Unterbrechung Einstellungen > transportieren Typ**, Azure-Dienstbus wählen Sie aus, und geben Sie die Werte aus, wie in der Abbildung dargestellt.

4.  Erstellen Sie den Vertrag empfangen zwischen Contoso und Northwind ein. Dieses Vertrags steuert, wie "Contoso" die Bestätigung von Northwind empfängt.

    1.  Klicken Sie auf **Einstellungen zu erhalten**.

    2.  Behalten Sie die Standardwerte auf den Registerkarten **Transport** und **Transformieren** .

    3.  Laden Sie das Schema **EFACT_4.1_CONTRL.xsd** auf der Registerkarte **Protokoll** unter dem Abschnitt **Schemas** hoch. Dieses Schema ist mit dem Beispielpaket verfügbar.

    4.  Erstellen Sie auf der Registerkarte **Routing** eines Filters, um sicherzustellen, dass nur Empfangsbestätigungen von Northwind an Contoso weitergeleitet werden. Klicken Sie unter **Routing Einstellungen**klicken Sie auf **Hinzufügen** , um die Weiterleitung Filter zu erstellen.

        ![][6]  
        1.  Geben Sie Werte für **Regelname**, **Routing Regel**und **Ziel weiterleiten** an, wie in der Abbildung dargestellt.

        2.  Klicken Sie auf **Speichern**.

    5.  Klicken Sie auf der Registerkarte **Routing** erneut Geben Sie an, wo angehaltene Empfangsbestätigungen (Empfangsbestätigungen, die während der Verarbeitung fehl) zu weitergeleitet werden. Legen Sie den Transporttyp Azure Service Bus, Zieltyp in der **Warteschlange**, Authentifizierungstyp weiterleiten zu **Access-Signatur freigegeben** (SAS), geben Sie die Verbindungszeichenfolge SAS für den Namespace Dienstbus, und geben Sie den Namen der Warteschlange als **unterbrochen**.

5.  Klicken Sie abschließend auf **Bereitstellen** , um den Vertrag bereitstellen. Beachten Sie die Endpunkte, in dem Senden und Empfangen von Vereinbarungen erhalten bereitgestellt.

    *   Beachten Sie auf der Registerkarte **Senden Einstellungen** unter **Eingehende URL**den Endpunkt aus. Zum Senden einer Nachricht von Contoso zu Northwind mithilfe der Bridge EDI senden, müssen Sie eine Nachricht an diesen Endpunkt senden.

    *   Notieren Sie auf der Registerkarte **Einstellungen erhalten** unter **Transport**den Endpunkt aus. Senden eine Nachricht von Northwind zu Contoso Bridge, erhalten die EDI verwenden, müssen Sie eine Nachricht an diesen Endpunkt senden.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Schritt 3: Erstellen Sie und Bereitstellen Sie des Projekts BizTalk-Dienste

Im vorherigen Schritt bereitgestellt EDI senden und empfangen Vereinbarungen Referenzmaterialien und EDIFACT Rechnungen Verarbeitungszeit. Diese Vereinbarungen können nur Verarbeiten von Nachrichten, die die Nachricht Standardschema EDIFACT entsprechen. Jedoch sendet pro dem Szenario für diese Lösung, Contoso eine Rechnung an Northwind in einem für eigene Schema. Ja, vor dem Senden der Nachricht zur Brücke EDI senden, müssen sie aus dem Schema für die standard EDIFACT Rechnung Schema transformiert werden. Das Projekt BizTalk Services EAI unterstützt, die.

Das Projekt BizTalk-Dienste, **InvoiceProcessingBridge**, die die Nachricht umwandelt ist auch Bestandteil der Stichprobe, die Sie heruntergeladen haben. Das Projekt umfasst die folgenden Elemente:

*   **INHOUSEINVOICE. XSD** – Schema der für Rechnung, die an Northwind gesendet wird.

*   **EFACT_D93A_INVOIC. XSD** – Schema der Rechnung standard EDIFACT.

*   **EFACT_4.1_CONTRL. XSD** – Schema der die Bestätigung EDIFACT, die Nordwind an Contoso sendet.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – die Transformation, die im Schema für Rechnung der Rechnung Standardschema EDIFACT zugeordnet ist.  

### <a name="create-the-biztalk-services-project"></a>Erstellen Sie das Projekt BizTalk-Dienste
1.  Klicken Sie in Visual Studio-Lösung erweitern Sie das Projekt InvoiceProcessingBridge, und öffnen Sie die Datei **MessageFlowItinerary.bcs** .

2.  Klicken Sie auf eine beliebige Stelle in den Zeichenbereich aus, und legen Sie die **URL der BizTalk-Dienst** im Eigenschaftenfeld, um den Namen Ihres Abonnements BizTalk-Dienste angeben. Beispielsweise `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Ziehen Sie eine **XML-unidirektionalen Bridge** aus der Toolbox auf den Zeichenbereich. Legen Sie die Eigenschaften der Brücke **Entität namens-** und **Relative** auf **ProcessInvoiceBridge**ein. Doppelklicken Sie auf **ProcessInvoiceBridge** , um die Bridge Konfiguration Oberfläche zu öffnen.

4.  Klicken Sie in im Feld **Nachricht Dateitypen** auf das Pluszeichen (**+**) Schaltfläche, um das Schema der eingehenden Nachricht anzugeben. Da die eingehende Nachricht für die EAI Brücke immer für Rechnung ist, legen Sie den **INHOUSEINVOICE**.

    ![][8]  
5.  Klicken Sie auf die **XML-Transformation** -Shape, und klicken Sie im Eigenschaftenfeld für die Eigenschaft **Karten** , klicken Sie auf das Auslassungszeichen (**...**). Klicken Sie im Dialogfeld **Karten Auswahl** wählen Sie die Datei **INHOUSEINVOICE_to_D93AINVOIC** Transformation aus, und klicken Sie dann auf **OK**.

    ![][9]  
6.  Kehren Sie zu **MessageFlowItinerary.bcs**und aus der Toolbox, und ziehen Sie einen **Externen Endpunkt bidirektionaler** rechts neben der **ProcessInvoiceBridge**. Legen Sie seine **Entitätsname** -Eigenschaft auf **EDIBridge**ein.

7.  Explorer-Lösung erweitern Sie die **MessageFlowItinerary.bcs** , und doppelklicken Sie auf die Datei **EDIBridge.config** . Ersetzen Sie den Inhalt von der **EDIBridge.config** mit den folgenden.

    > [AZURE.NOTE] Warum muss ich die config-Datei bearbeiten? Der externe Endpunkt, den wir den Bridge-Designer Zeichenbereich hinzugefügt stellt die EDI Brücken, die wir zuvor bereitgestellt. EDI Brücken sind bidirektionale Brücken, mit einem senden und Empfangen von Seite. Die EAI Brücke, die wir den Bridge-Designer hinzugefügt ist jedoch eine unidirektionale Verbindung. Ja, verwenden, um die verschiedenen Nachricht Exchange Muster von zwei Brücken zu behandeln wir eine benutzerdefinierte Bridge Verhalten indem seine Konfiguration in der config-Datei. Darüber hinaus übernimmt das benutzerdefinierte Verhalten auch die Authentifizierung an den Endpunkt EDI senden Bridge aus. Dieses Verhalten benutzerdefinierte steht in einem separaten Beispiel am [BizTalk Services Bridge verketten Sample - EAI EDI](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Diese Lösung verwendet die Stichprobe wieder.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Aktualisieren Sie die Datei EDIBridge.config Details zur Konfiguration aufnehmen möchten.

    *   Klicken Sie unter _<behaviors>_, steht die ACS-Namespace und den Schlüssel, mit dem BizTalk-Dienste-Abonnement verknüpft ist.

    *   Klicken Sie unter _<client>_, geben Sie den Endpunkt, in dem der EDI senden Vertrag bereitgestellt wird.

    Speichern Sie Änderungen zu und schließen Sie die Konfigurationsdatei.

9.  Klicken Sie auf den **Verbinder** , und teilnehmen an die Komponenten **ProcessInvoiceBridge** und **EDIBridge** , aus der Toolbox. Wählen Sie den Verbinder aus, und legen Sie im Eigenschaften **Filter Bedingung** auf **Alle Übereinstimmung**. Dadurch wird sichergestellt, dass alle Nachrichten verarbeitet wurden durch die EAI Brücke zur Brücke EDI weitergeleitet werden.

    ![][10]  
10.  Speichern von Änderungen in der Lösung.  

### <a name="deploy-the-project"></a>Bereitstellen des Projekts

1.  Klicken Sie auf dem Computer, in dem Sie das Projekt BizTalk-Dienste erstellt haben, herunterladen Sie und installieren Sie das SSL-Zertifikat für Ihr Abonnement BizTalk-Dienste. Aus unter BizTalk-Dienste, klicken Sie auf **Dashboard**, und klicken Sie dann auf **SSL-Zertifikat herunterladen**. Doppelklicken Sie auf das Zertifikat, und folgen Sie aufgefordert werden, die Installation durchzuführen. Stellen Sie sicher, dass Sie das Zertifikat unter **Vertrauenswürdige** Zertifikat Serverzertifikat installieren.

2.  In Visual Studio Solution Explorer mit der rechten Maustaste in des Projekts **InvoiceProcessingBridge** , und klicken Sie dann auf **Bereitstellen**.

3.  Geben Sie die Werte aus, wie in der Abbildung dargestellt, und klicken Sie dann auf **Bereitstellen**. Sie können die ACS-Anmeldeinformationen für BizTalk-Dienste abrufen, indem Sie **Verbindungsinformationen** aus dem Dashboard BizTalk-Dienste auf.

    ![][11]  

    Kopieren Sie den Endpunkt, in dem die EAI Bridge, beispielsweise bereitgestellt wird, aus dem Ausgabebereich `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Sie benötigen diese Endpunkt-URL später.  

## <a name="step-4-test-the-solution"></a>Schritt 4: Testen der Lösung


In diesem Thema betrachten wir so testen Sie die Lösung mithilfe der **Lernprogramm** Clientanwendung liegen als Teil der Stichprobe.  

1.  Drücken Sie F5, um den **Client Lernprogramm**starten in Visual Studio.

2.  Der Bildschirm müssen die Werte aus dem Schritt bereits ausgefüllt, in dem wir die Servicebuswarteschlangen erstellt. Klicken Sie auf **Weiter**.

3.  Geben Sie im nächsten Fenster ACS-Anmeldeinformationen für BizTalk-Dienste-Abonnement und die Endpunkte des, wo EAI und EDI (erhalten) Brücken bereitgestellt werden.

    Sie haben den EAI Bridge Endpunkt im vorherigen Schritt kopiert haben. Erhalten Sie für EDI Bridge-Endpunkt im Portal BizTalk, wechseln Sie zu der Vereinbarung > Einstellungen erhalten > Transport > Endpunkt.

    ![][12]  
4.  Im nächsten Fenster klicken Sie unter "Contoso", auf die Schaltfläche **Für Rechnung senden** . Klicken Sie in der Datei öffnen Sie im Dialogfeld, öffnen Sie die Datei INHOUSEINVOICE.txt. Überprüfen Sie den Inhalt der Datei, und klicken Sie dann auf **OK** , um die Rechnung zu senden.

    ![][13]  
5.  Die Rechnung wird in ein paar Sekunden bei Northwind empfangen. Klicken Sie auf die **Nachricht anzeigen** -Link, um die Rechnung vom Northwind finden Sie unter. Beachten Sie, dass die Rechnung vom Northwind im standard EDIFACT Schema während gesendet werden, indem Sie Contoso ein Schema für wurde.

    ![][14]  
6.  Wählen Sie die Rechnung aus, und klicken Sie dann auf **Bestätigung zu senden**. Klicken Sie im Dialogfeld, das eingeblendet wird, beachten Sie, dass die Interchange-ID derselben in empfangenen Rechnung und die Bestätigung gesendet werden. Klicken Sie auf OK, klicken Sie im Dialogfeld **Bestätigung zu senden** .

    ![][15]  
7.  In ein paar Sekunden wird die Bestätigung erfolgreich bei Contoso empfangen.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Schritt 5 (optional): Senden EDIFACT Rechnung in Stapeln 
BizTalk Services EDI Brücken unterstützt auch Batchverarbeitung von ausgehenden Nachrichten. Diese Funktion eignet sich für den Empfang von Partnern, die eine Reihe von Nachrichten (Besprechung bestimmte Kriterium) statt einzelne Nachrichten erhalten möchten.

Der wichtigste Aspekt bei der Arbeit mit Blattnamen ist der tatsächliche Version des Stapels, auch als die veröffentlichte Version Kriterien bezeichnet. Die veröffentlichte Version Kriterien können auf wie der empfangende Partner Nachrichten empfangen möchte basieren. Wenn die Batchverarbeitung aktiviert ist, sendet die Brücke EDI ausgehenden Nachrichten erst an den Partner empfangen an, wenn die Release-Kriterien erfüllt ist. Beispielsweise eine Batchverarbeitung Kriterien Grundlage Nachricht Größe sendet einen Stapel nur, wenn "n" Nachrichten zusammengefasst werden. Kriterien für die Stapelverarbeitung können auch zeitbasierte, sein, ein Stapel zu einer festen Zeit täglich gesendet wird. In dieser Lösung versuchen wir die Größe der Nachrichten basierend Kriterien aus.

1.  Klicken Sie im Portal BizTalk auf den zuvor erstellten Vertrag. Klicken Sie auf Senden-Einstellungen > Batchverarbeitung > Stapel hinzufügen.

2.  Geben Sie für Blattname **InvoiceBatch**, geben Sie eine Beschreibung, und klicken Sie dann auf **Weiter**.

3.  Angeben einer Kriterien für die Stapelverarbeitung, die definiert, welche Nachrichten zusammengefasst werden müssen. In dieser Lösung Stapel wir alle Nachrichten aus. Ja, aktivieren Sie das erweiterte Definitionen Option verwenden, und geben Sie **1 = 1**. Dies ist eine Bedingung, die immer true genommen wird, und daher für alle Nachrichten zusammengefasst werden. Klicken Sie auf **Weiter**.

    ![][17]  
4.  Geben Sie die Kriterien für die Veröffentlichung einer Stapelverarbeitung ein. Klicken Sie aus dem Dropdownfeld wählen Sie **MessageCountBased**aus, und geben Sie für **Anzahl**, **3**. Dies bedeutet, dass eine Reihe von drei Nachrichten an Northwind gesendet wird. Klicken Sie auf **Weiter**.

    ![][18]  
5.  Überprüfen Sie die Zusammenfassung, und klicken Sie dann auf **Speichern**. Klicken Sie auf **Bereitstellen** , um den Vertrag erneut bereitstellen.

6.  Kehren Sie zu dem **Lernprogramm Client**, klicken Sie auf **Für Rechnung senden**, und folgen Sie den Anweisungen, die Rechnung senden. Beachten Sie, dass keine Rechnung am Northwind empfangen werden, da die Stapelgröße nicht erfüllt ist. Wiederholen Sie diesen Schritt noch zweimal, dass Sie drei Rechnungsnachrichten an Northwind gesendet haben. Dies erfüllt die Kriterien für die Stapelverarbeitung Release 3 Nachrichten und eine Rechnung am Northwind sollte jetzt angezeigt werden.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

