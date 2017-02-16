<properties 
    pageTitle="Erfahren Sie mehr über Enterprise Integration Pack entschlüsseln EDIFACT Nachricht Verbinder | Microsoft Azure-App-Verwaltungsdienst | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Partner mit der apps Enterprise Integration Pack und Logik" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-edifact-message"></a>Erste Schritte mit EDIFACT Nachricht entschlüsseln

Überprüft, ob EDI und Partner-spezifische Eigenschaften, generiert XML-Dokument für jede Transaktion und Bestätigung für verarbeiteten Transaktion.

## <a name="create-the-connection"></a>Herstellen der Verbindungs

### <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Entschlüsseln EDIFACT Nachricht Verbinder verwenden, ist eine Integration-Konto erforderlich. Anzeigen von Details zum Erstellen einer [Integration-Konto](./app-service-logic-enterprise-integration-create-integration-account.md), [Partnern](./app-service-logic-enterprise-integration-partners.md) und [EDIFACT Vertrag](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Verbinden Sie mit entschlüsseln EDIFACT-Nachricht mithilfe der folgenden Schritte:

1. [Erstellen einer App Logik](./app-service-logic-create-a-logic-app.md) enthält ein Beispiel.

2. Alle Trigger keinen dieser Verbinder. Verwenden Sie andere Trigger, um die App Logik, wie z. B. eine Anforderung Trigger starten.  Hinzufügen eines Triggers im Logik App-Designer und eine Aktion hinzufügen.  Wählen Sie Microsoft anzeigen verwalteten APIs in der Dropdown-Liste, und geben Sie "EDIFACT" in das Suchfeld ein.  Wählen Sie entschlüsseln EDIFACT-Nachricht

    ![EDIFACT suchen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Wenn Sie alle Verbindungen mit Integration Konto zuvor erstellt haben, werden Sie für die Details der Verbindung aufgefordert.

    ![Erstellen Sie Integration-Konto](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Geben Sie die Details der Integration-Konto an.  Eigenschaften mit einem Sternchen sind erforderlich

  	| Eigenschaft | Details |
  	| -------- | ------- |
  	| Verbindungsnamen * | Geben Sie einen beliebigen Namen für die Verbindung |
  	| Integration Konto * | Geben Sie den Namen des Kontos Integration. Achten Sie darauf, dass Ihr Integration-Konto und Logik app an derselben Stelle Azure sind |

    Sobald Sie fertig sind, suchen Sie die Verbindungsdetails ähnlich wie der folgende

    ![Integration Konto erstellt](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Wählen Sie **Erstellen**

6. Beachten Sie, dass die Verbindung erstellt wurde

    ![Informationen zur Integration Konto-Verbindung](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Wählen Sie EDIFACT Flatfile Nachricht entschlüsseln

    ![Bereitstellen von Pflichtfelder](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT entschlüsseln bedeutet folgen

* Beheben Sie den Vertrag durch den Abgleich der Absender Kennung und Bezeichner und Empfänger Kennung und Bezeichner
* Teilt mehrere Austauschvorgänge in einer einzelnen Nachricht in Separate an.
* Überprüft den Umschlag gegen trading Partner-Vertrag
* Löst das Interchange an.
* Überprüft, ob EDI und Partner-spezifische Eigenschaften enthält
    * Überprüfung von die Struktur des Umschlags Interchange.
    * Schema Überprüfung des Umschlags gegen das Schema steuern.
    * Schema Überprüfung der Elemente Transaktion-Set-Daten gegen das Nachrichtenschema.
    * Überprüfung auf Datenelemente Transaktion-Set EDI
* Überprüft, ob die Zahlen Interchange, Gruppen- und Transaktion festlegen Steuerelement nicht Duplikate (falls konfiguriert) 
    * Überprüft die Interchange-Steuerelement-Nummern mit zuvor empfangenen Austauschvorgänge. 
    * Überprüft die Gruppe Steuerelement Anzahl für andere Gruppe Steuerelement Zahlen im Austausch. 
    * Überprüft, dass die Transaktion Steuerelement Anzahl für andere Transaktion festlegen Steuerelement Zahlen in dieser Gruppe festzulegen.
* Generiert ein XML-Dokument für jede Transaktion.
* Wandelt die gesamte Interchange das Arbeiten mit XML 
    * Geteilte Interchange als Transaktionssätze - aussetzen Transaktionssätze Fehler: analysiert jede Transaktion ein Austausch in einem separaten XML-Dokument festlegen. Wenn Sie einen oder mehrere Transaktionssätze im Austausch, fehlgeschlagener Überprüfung und dann nur diese Transaktion Mengen EDIFACT entschlüsseln ausgesetzt werden. 
    * Geteilte Interchange als Transaktionssätze - aussetzen Interchange Fehler: analysiert jede Transaktion ein Austausch in einem separaten XML-Dokument festlegen.  Wenn Sie einen oder mehrere Transaktionssätze im Austausch, fehlgeschlagener Überprüfung und dann die gesamte Interchange EDIFACT entschlüsseln ausgesetzt werden.
    * Beibehalten Interchange - aussetzen Transaktionssätze Fehler: ein XML-Dokument für den Austausch des gesamten gespeicherten erstellt. Nur diese Transaktion Mengen, die fehlgeschlagener Überprüfung, und alle anderen Gruppen Transaktion Verarbeitungszeit weiterhin die EDIFACT entschlüsseln ausgesetzt
    * Beibehalten Interchange - aussetzen Interchange Fehler: ein XML-Dokument für den Austausch des gesamten gespeicherten erstellt. Wenn Sie einen oder mehrere Transaktionssätze im Austausch, fehlgeschlagener Überprüfung und dann die gesamte Interchange EDIFACT entschlüsseln ausgesetzt werden, 
* (Falls konfiguriert) technischen (Steuerelement) und/oder ein funktionsübergreifendes Bestätigung generiert werden soll.
    * Eine technische Bestätigung oder das CONTRL ACK Berichte die Ergebnisse einer Syntax Prüfung der Fertigstellung empfangenen Austausch.
    * Eine Bestätigung funktionsübergreifendes bestätigt annehmen oder Ablehnen einer empfangenen Interchange oder einer Gruppe

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über das Enterprise-Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Pack für Enterprise-Integration") 