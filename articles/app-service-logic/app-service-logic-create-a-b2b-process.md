<properties 
   pageTitle="Erstellen eines Prozesses B2B in Azure-App-Verwaltungsdienst | Microsoft Azure" 
   description="So erstellen Sie einen Business-to-Business-Prozess im Überblick" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Erstellen eines Prozesses B2B

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Szenario für Unternehmen 
Contoso und Northwind sind zwei Business-Partner zur Verfügung. Contoso (Einzelhändler) sendet Bestellungen auf Northwind (Lieferanten) über einen Transport Industry Ebene, wie z. B. AS2. Northwind speichert alle eingehenden Aufträge in seinem Cloud-Speicher. Die Bestellung handelt es sich um XML-Nachrichten zwischen diesen zwei Partner zur Verfügung. Sobald die Nachricht in Northwinds-Cloud-Speicher gespeichert wurde verarbeitet Northwind interne Prozesse die Reihenfolge von diesem Punkt auf.
 
Ziel dieses Lernprogramms ist, um festzulegen, wie Northwind einen Geschäftsprozess herstellen kann, über dem sie empfangen (Bestellungen in XML-Datei) aus dem Contoso-Partner über AS2 und es dann in der Cloud-Speicher beibehalten werden kann.


## <a name="capabilities-demonstrated"></a>Funktionen gezeigt 
In diesem Lernprogramm hilft präsentieren Sie die folgenden Funktionen: 

- **Shapes für Transport Nachricht**: die Einzelhändler und Lieferanten können auf unterschiedlichen Plattformen werden, jedoch können sie dennoch Kommunikation zwischen den beiden erreichen. In diesem Lernprogramm kommunizieren sie über AS2 (Anwendungsmöglichkeiten Anweisung 2). AS2 ist eine beliebte Möglichkeit, die Übertragung von Daten zwischen Partnern in Business-to-Business-Kommunikation.
- **Beibehaltung von Daten**: Nachdem Sie die Nachricht dann Northwind belassen Sie dieses möchte, bevor weitere Verarbeitung über AS2 empfangen wurde. Sie können einen Verbinder verwenden, um Nachrichten in der Cloud-Speicher beizubehalten. In diesem Lernprogramm wird Azure Blobs als Cloud-Speicher für Northwind genutzt wird.
- **Erstellen Sie einen Geschäftsprozess**: In einem Datenfluss, mehrere API apps können werden so geändert, zusammen, wenn Sie eine Business Ergebnis erzielen, wie hier gezeigt.


## <a name="before-you-begin"></a>Vorbemerkung
In diesem Lernprogramm wird davon ausgegangen, dass Sie wissen, wie Sie apps API erstellen und einen Fluss zusammen zusammenzufügen, grundlegende Kenntnisse Azure App Services haben.


## <a name="steps-to-achieve-the-business-scenario"></a>Schritte zum Business-Szenario zu erzielen.
**Erstellen Sie und konfigurieren Sie die erforderlichen API-apps**

1. Erstellen Sie eine Instanz der **Azure-Speicher Blob Verbinder**. Setzt die Anmeldeinformationen für ein Konto Azure-Speicher. Stellen Sie sicher, dass er bereit ist, bevor Sie beginnen, dies zu erstellen.
2. Erstellen Sie eine Instanz der **BizTalk Trading Partner Management**. Setzt eine leere SQL-Datenbank zu (Funktion). Stellen Sie sicher, dass er bereit ist, bevor Sie beginnen, dies zu erstellen.
3. Erstellen Sie eine Instanz des **AS2 Verbinder**. Setzt auch eine leere SQL-Datenbank zu (Funktion). Stellen Sie sicher, dass er bereit ist, bevor Sie beginnen, dies zu erstellen. Darüber hinaus, wenn Sie Nachrichten als Teil AS2 Verarbeitung archivieren möchten, können Sie Anmeldeinformationen in einer Azure Blob bei seiner Erstellung angeben.
4. Konfigurieren des TPM (Trading Partner Management)-Diensts, der erstellt wurde:  
    1. Navigieren Sie zu der Instanz des Diensts TPM als Teil der oben genannten Schritte erstellt.
    2. Verwenden Sie die Option **Partner** unter *Komponenten* zum **Hinzufügen** einer neuen Partner mit dem Namen **"Contoso"** und in seinem Profil hinzufügen die erforderliche AS2 Identität.
    3. Verwenden Sie die Option **Partner** unter *Komponenten* zum **Hinzufügen** einer neuen Partner mit dem Namen **Nordwind** und deren Profil hinzufügen die erforderliche AS2 Identität.
    4. Mithilfe der Option **Vereinbarungen** unter *Komponenten* zum **Hinzufügen** einer neuen AS2 Vertrag zwischen Northwind und Contoso. Northwind werden hier die gehosteten Partner und Contoso werden auf dem Gast-Partner. Je nach Bedarf bei der Anmeldung Verschlüsselung, Komprimierung und Empfangsbestätigungen während dieser Vertrag Erstellung konfigurieren. Für den Fall, dass Zertifikate verwendet werden müssen, können diese über die Option **Zertifikate** hochgeladen werden beim Durchsuchen von des TPM-Diensts, der erstellt wird.


## <a name="create-a-flow--business-process"></a>Erstellen Sie einen Fluss / Business Prozess
1. Erstellen eines neuen Fluss in dem im ersten Schritt AS2 wird. Ziehen Sie und legen Sie der **AS2-Connector ab** , und wählen Sie die Instanz, die bereits erstellt. Wählen Sie als die Funktionalität Trigger aus:  
    ![][1]  
2. Ziehen Sie als Nächstes und legen Sie **Azure Speicher Blob Verbinder ab** , und wählen Sie die Instanz, die bereits erstellt haben. Wählen Sie die Aktion als die Funktionalität und in diesem, wählen Sie als die gewünschte Funktionalität **Blob hochladen** aus. Führen Sie die entsprechenden.
3. Nun erstellen/Bereitstellen des Ablaufs.


## <a name="message-processing--troubleshooting"></a>Verarbeitung von Nachrichten und Problembehandlung
1. Es ist Zeit zum Testen des Ablaufs, die wir bereitgestellt haben. Send XML in Umgebrochene AS2 (gemäß dem AS2-Vertrag oben erstellte) Nachrichten an den AS2 Endpunkt angegeben werden, indem Sie die AS2Connector-Instanz, die Sie erstellt haben. Möglicherweise müssen Sie die Authentifizierung für den Endpunkt so konfigurieren, dass es öffentlich zugänglich ist.
2. Von Ausführungsinformationen zur Übertragung wird angegeben, indem zu illustrieren navigieren, und klicken Sie dann auf die Fluss-Instanz, die auf Ihrem System ausgeführt schrittweises
3. Navigieren Sie zu der AS2Connector Instanz verbindet AS2 Verarbeitung von Informationen, und folgen Sie, indem Sie das Webpart Verlauf schrittweises. Sie können die Filter, die zur Verwaltung die Ansicht auf die Informationen zu beschränken, die gewünscht wird.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
