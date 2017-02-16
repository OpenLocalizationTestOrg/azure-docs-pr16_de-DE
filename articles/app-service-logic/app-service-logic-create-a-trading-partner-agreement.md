<properties 
   pageTitle="Erstellen Sie eine separate Partner Vereinbarung treffen in Azure App-Verwaltungsdienst | Microsoft Azure" 
   description="Erstellen von Trading Partner Agreements" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Erstellen eine separate Partner Vereinbarung treffen   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Partnern sind die Personen beteiligt B2B Kommunikation (Business-to-Business). Wenn Sie zwei Partner eine Beziehung erstellen möchten, wird dies als *Vertrag*bezeichnet. Der Vertrag definiert basiert auf der Kommunikation der beiden Partner langfristig zu erzielen und Protokoll oder eine bestimmte Übertragung ist. Die verschiedenen B2B Protokolle und Transport von App-Verwaltungsdienst Azure unterstützt umfassen:

- AS2 (Anwendungsmöglichkeiten Anweisung 2)
- EDIFACT (un/elektronische Data Interchange für Verwaltung, Handel und Transport (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk-API Apps, die B2B Szenarien unterstützen
Die folgenden API Apps aktivieren Sie diese Funktionen, die mit einer leistungsfähigen und intuitive Benutzeroberfläche im Portal Azure:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk Trading Partner Management (TPM)
- Erstellen und Verwalten von Partnern, Profile und Identitäten
- Speicherung und Verwaltung von EDI-Schemas
- Speicherung und Verwaltung von Zertifikaten (AS2 Protocol)
- Erstellen und Verwalten von AS2 Agreements
- Erstellen und Verwalten von EDIFACT Agreements (einschließlich Batchverarbeitung auf der Sendeseite)
- Erstellen und Verwalten von X12 Agreements (einschließlich Batchverarbeitung auf der Sendeseite)

![][1]


## <a name="as2-connector"></a>AS2-Connector
- AS2 Vereinbarungen ausgeführt wird, wie in der zugehörigen TPM API App-Instanz definiert sind.
- Flächen AS2 Verarbeitung/Verlauf Informationen zur Behandlung dieses Problems


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- EDIFACT Vereinbarungen ausgeführt wird, wie in der zugehörigen TPM API App-Instanz definiert sind.
- Flächen EDIFACT Verarbeitung/Verlauf Informationen zur Behandlung dieses Problems
- Bundesstaat Verwaltung von Blattnamen (Start- und) enthält, wie in der zugehörigen TPM API App-Instanz in EDIFACT Vereinbarung(en) definiert


## <a name="biztalk-x12"></a>BizTalk X12
- Führt X12 Vereinbarungen wie in der zugehörigen TPM API App-Instanz definiert sind. 
- Flächen X12 Verarbeitung/Verlauf Informationen zur Behandlung dieses Problems
- Bundesstaat Verwaltung von Blattnamen (Start- und) enthält, wie in X12 definiert Vereinbarung(en) in der zugehörigen TPM API App-Instanz

Wie zuvor erwähnt wird die AS2, X 12 und EDIFACT API Apps einer Funktion App-API TPM erforderlich wie erwartet.


## <a name="getting-started"></a>Erste Schritte
So erstellen Sie Handel Partner agreements

1. Erstellen Sie eine Instanz des Verbinders **BizTalk Trading Partner Management** . Setzt eine leere SQL-Datenbank zu (Funktion). Bevor Sie beginnen Sie sicher, dass eine leere Datenbank zur Verfügung und können verwendet werden.
2. Hochladen von Schemas und Zertifikate nach Bedarf von den Verträgen. Zu diesem Zweck die TPM-Instanz erstellt und das Webpart 'Schemas' und/oder 'Zertifikate' schrittweises durchsuchen
3. Navigieren Sie zu der TPM-Instanz erstellt und Einzelschritt in das Webpart für **Partner**
4. Partner nach Bedarf zu erstellen. Auch die Profile nach Bedarf bearbeiten und Hinzufügen der erforderlichen Identitäten
5. Nun mithilfe des Webparts **Vereinbarungen** um Vereinbarungen zu erstellen. Wenn Sie einen Vertrag erstellen, müssen Sie das Protokoll auswählen, das verwendet werden soll. Die übrigen Konfigurationsoptionen basieren auf das Protokoll, die Sie ausgewählt haben.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
