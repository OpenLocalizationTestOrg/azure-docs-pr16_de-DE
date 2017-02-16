<properties
    pageTitle="Übersicht über Schemas und die Enterprise-Integration Pack | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Schemas mit der apps Enterprise Integration Pack und Logik"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Erfahren Sie mehr über Schemas und das Paket für die Enterprise-Integration  

## <a name="why-use-a-schema"></a>Warum ist verwenden ein Schema?
Verwenden Sie Schemas, um zu bestätigen, dass XML-Dokumente, die Sie erhalten, mit der erwarteten Daten in einem vordefinierten Format gültig sind. Schemas werden verwendet, um Nachrichten zu überprüfen, die in einem Szenario B2B ausgetauscht werden.

## <a name="add-a-schema"></a>Fügen Sie ein schema
Vom Azure-Portal:  

1. Wählen Sie **Weitere Dienste**.  
![Screenshot der Azure-Portal mit hervorgehobenem "Weitere Dienste"](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. Klicken Sie in das Suchfeld der Filters Geben Sie **Integration ein**, und wählen Sie aus der Ergebnisliste **Integration-Konten** .     
![Screenshot des Suchfelds filtern](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Wählen Sie die **Integration-Konto** , dem Sie das Schema hinzufügen.    
![Screenshot der Integration Kontenliste](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Wählen Sie die Kachel **Schemas** aus.  
![Screenshot der IEP Integration-Konto mit "Schemas" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Hinzufügen einer Schemadatei kleiner als 2 MB  

1. Wählen Sie in den **Schemas** Blade, das (aus den vorherigen Schritten) geöffnet wird **Hinzufügen**aus.  
![Screenshot der Schemas Blade, mit der Schaltfläche "Hinzufügen", die hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Geben Sie einen Namen für Ihr Schema aus. Wählen Sie dann das Ordnersymbol neben dem Textfeld **Schema** zum Hochladen der Schemadatei aus. Nachdem der Uploadvorgang abgeschlossen ist, wählen Sie **OK**aus.    
![Screenshot "Schema hinzufügen", mit "Klein" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Hinzufügen einer Schemadatei größer als 2 MB (bis zu einem Maximum von 8 MB)  

Die Verarbeitung hierfür hängt die Blob Container Zugriffsebene: **Öffentliche** oder **keine anonymen Zugriff**. Wählen Sie den gewünschten Blob-Container diese Zugriffsebene, im **Speicher-Explorer Azure** **Blob-Container**, klicken Sie unter ermitteln. Wählen Sie **Sicherheit**, und wählen Sie die Registerkarte **Zugriffsebene** aus.

1. Wenn Access die Blob-Sicherheitsstufe **öffentlich**ist, gehen Sie folgendermaßen vor.  
  ![Screenshot der Azure-Speicher-Explorer mit "Blob Container", "Sicherheit" und "Öffentliche" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    ein. Hochladen Sie das Schema zu Speicher, und kopieren Sie den URI.  
    ![Screenshot des Speicherkonto mit hervorgehobenem URI](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. Wählen Sie im **Schema hinzufügen** **große Datei**, und Bereitstellen Sie den URI im Textfeld **URI Inhalte** .  
    ![Screenshot des Schemas, mit der Schaltfläche "Hinzufügen" und "Große Datei" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Wenn Access die Blob-Sicherheitsstufe **keine anonymer Zugriff**ist, gehen Sie folgendermaßen vor.  
  ![Screenshot der Azure-Speicher-Explorer mit "Blob Container", "Sicherheit" und "Kein anonymer Zugriff" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    ein. Laden Sie das Schema auf Speicher.  
    ![Screenshot des Speicher-Konto](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Generieren einer freigegebenen Access-Signatur für das Schema an.  
    ![Screenshot des Benutzerkontos Speicher, mit der gemeinsamen Zugriff Signaturen Registerkarte hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. Wählen Sie in **Schema hinzufügen** **große Datei**und geben Sie die gemeinsamen Zugriff Signatur in das Textfeld **Inhalt URI** URI an.  
    ![Screenshot des Schemas, mit der Schaltfläche "Hinzufügen" und "Große Datei" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. Falz **Schemas** das EIP Integration-Konto sollten Sie jetzt neu hinzugefügte Schema angezeigt.  
![Screenshot der EIP Integration-Konto mit "Schemas", und das neue Schema hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Bearbeiten von schemas
1. Wählen Sie die Kachel **Schemas** aus.  
2. Wählen Sie das Schema aus dem Blade **Schemas** bearbeiten, die geöffnet werden soll.
3. Klicken Sie auf das Blade **Schemas** wählen Sie **Bearbeiten**aus.  
![Screenshot der Schemas blade](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Wählen Sie die Schemadatei aus, die Sie mithilfe der Datei Datumsauswahl im daraufhin angezeigten Dialogfeld bearbeiten möchten.
5. Wählen Sie im Auswahltool für Datei auf **Öffnen** .  
![Screenshot der Datei Datumsauswahl](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Sie erhalten eine Benachrichtigung, die angibt, dass der Upload erfolgreich war.  

## <a name="delete-schemas"></a>Löschen von schemas
1. Wählen Sie die Kachel **Schemas** aus.  
2. Wählen Sie das Schema aus dem Blade **Schemas** löschen, die geöffnet werden soll.  
3. Klicken Sie auf das Blade **Schemas** wählen Sie **Löschen**aus.
![Screenshot der Schemas blade](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Wählen Sie **Ja**, um die Auswahl zu bestätigen.  
![Screenshot der Meldung zur Bestätigung der "Schema löschen"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Beachten Sie, dass die Liste der Schemas in das Blade **Schemas** aktualisiert, und das Schema aus, das Sie gelöscht haben, wird nicht mehr aufgelistet.  
![Screenshot der EIP Integration-Konto mit "Schemas" hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über das Enterprise-Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über das Enterprise-Integration Pack").  
