<properties 
    pageTitle="Business-to-Business Verbindern und API Apps Logik Apps | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Konfigurieren von EDI, EDIFACT, AS2 und TPM Verbinder; Microservices Architektur" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business-to-Business Verbindern und API-Apps

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logik Apps enthält viele BizTalk-API Apps, die Integration Umgebungen wichtig sind. Diese API Apps basieren auf Konzepte und Tools in BizTalk Server verwendet, aber stehen nun als Teil der Logik Apps. 

Eine Kategorie von dieser API Apps sind Business-to-Business (B2B) API Apps. Mithilfe dieser B2B-API Apps, können Sie ganz einfach Partner hinzufügen, Vereinbarungen erstellen, und führen Sie alles wie würde lokal mit EDI, AS2 und EDIFACT.  

Diese B2B-API Apps bieten "Trigger" und "Aktion" Funktionen aus. Ein Triggers startet eine neue Instanz, die ein bestimmtes Ereignis, wie das Eintreffen einer X12 ausgehend von einem Partner Nachricht. Eine Aktion ist das Ergebnis, wie nach Erhalt einer X12 Nachricht, und senden Sie die Nachricht mit AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Was ist ein Business-to-Business-Verbinder oder API Apps
Das Feature Business-to-Business (B2B) enthält die vorhandenen, vordefinierten API-Apps, die anderen Unternehmen, Abteilungen, Anwendungen usw. zum Kommunizieren mit AS2, EDI und EDIFACT zulassen. 

Die B2B-API Apps gehören: 

Verbinder oder API-Apps | Beschreibung
--- | ---
BizTalk Trading Partner Management | Eine API-App, die Business-to-Business (B2B) Beziehungen mit Partner und Vereinbarungen erstellt. Diese Beziehungen zu nutzen, die AS2, EDIFACT und X12 Protokoll.<br/><br/>Die App TPM-API ist die Basis Vorbedingung AS2-Connector, und die X12 oder EDIFACT-API Apps. 
AS2-Connector | Verbinder, der empfängt und sendet Nachrichten über den Transport AS2. Der Verbinder übermittelt Daten zuverlässig und sicher über das Internet an.
BizTalk EDIFACT | Eine API-App, die empfängt und sendet Nachrichten mithilfe von EDIFACT. EDIFACT wird häufig auch als UN/EDIFACT (un/Electronic Data Interchange For Administration, Handel und Verkehr) bezeichnet und in Branchen weit verbreitet ist.
BizTalk X12 | Eine API-App, die empfängt und sendet Nachrichten mithilfe der X12 Protokoll. X12 wird häufig auch als ASC X 12 (autorisierten Standards Committee X12) bezeichnet und in Branchen weit verbreitet ist. 


Diese API Apps können Sie verschiedene EDI messaging Aufgaben abschließen. Angenommen, mithilfe von AS2 Netzwerke, Sie können sicher erhalten und verschiedene Arten von Nachrichten senden (XML, EDI Flatfile usw.) zu einem Kunden, wie eine Division innerhalb des Unternehmens Personalwesen oder jede Person, die AS2 verwendet. 

Sie können beliebig viele API Apps soll, und erstellen sie einfach erstellen. Sie können auch eine einzelne API App in mehreren Szenarien oder Workflows wiederverwenden.

Sie können ohne Code ausführen. Lassen Sie uns beginnen. 


## <a name="requirements-to-get-started"></a>Anforderungen zur Seite Erste Schritte
Wenn Sie B2B-API Apps erstellen, gibt es erforderlichen Ressourcen. Diese Elemente müssen von Ihnen erstellt werden, bevor sie in andere Apps-API verwendet werden können. Diese Anforderungen umfassen: 

Anforderung | Beschreibung
--- | ---
SQL Azure-Datenbank | Speichert B2B Elemente, einschließlich Partner, Schemas, Zertifikate und Agreeements. Jeder der Apps-API B2B erfordert eine eigene Azure SQL-Datenbank. <br/><br/>**Notiz** Kopieren Sie die Verbindungszeichenfolge in dieser Datenbank ein.<br/><br/>[Erstellen einer SQL Azure-Datenbank](../sql-database/sql-database-get-started.md)
Azure Blob-Speicher container | Stores Meldung Eigenschaften AS2 Archivierung aktiviert ist. Wenn Sie zum Archivieren von Nachrichten AS2 benötigen, ist kein Containers Speicher erforderlich. <br/><br/>**Notiz** Wenn Sie die Archivierung aktivieren möchten, kopieren Sie die Verbindungszeichenfolge in dieser Blob-Speicher.<br/><br/>[Informationen über Konten Azure-Speicher](../storage/storage-create-storage-account.md)
Service Bus Namespace und deren Werte für Schlüssel | Speichert X12 und EDIFACT Batchverarbeitung von Daten. Wenn Sie die Batchverarbeitung benötigen, ist ein Dienstbus Namespace nicht erforderlich.<br/><br/>**Notiz** Wenn Sie die Batchverarbeitung aktivieren möchten, kopieren Sie diese Werte.<br/><br/>[Erstellen eines Service Bus Namespace](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM-Instanz | Eine Instanz BizTalk Trading Partner Management (TPM) ist erforderlich, um die AS2-Connector und X12 oder EDIFACT-API App erstellen. Wenn Sie die TPM-API App erstellen, erstellen Sie die TPM-Instanz aus. <br/><br/>**Notiz** Kennen Sie den Namen der App-API TPM. 


## <a name="create-the-api-apps"></a>Erstellen der API-Apps
B2B-API Apps können mithilfe des Azure-Portals oder mithilfe von REST-APIs erstellt. 


### <a name="create-the-api-apps-using-rest-apis"></a>Erstellen der restlichen APIs mit API-Apps
[Finden Sie in der Dokumentation zur Verwendung der restlichen APIs.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Erstellen der B2B-API Apps im Azure-Portal
Im Portal Azure können Sie B2B-API Apps erstellen, wenn Logik Apps, Web Apps oder Mobile-Apps zu erstellen. Alternativ können Sie eine mit einem eigenen Blade erstellen. Beide Methoden sind einfach, sodass sie Ihren Anforderungen oder Voreinstellungen abhängig. Einige Benutzer bevorzugen alle B2B-API Apps zuerst mit ihren bestimmten Eigenschaften zu erstellen. Klicken Sie dann erstellen Sie den Logik Apps/Web Apps/Mobile-Apps und fügen Sie der B2B-API Apps hinzu, die Sie erstellt haben.  

Die folgenden Schritte Erstellen der B2B-API Apps mithilfe des Apps-API Blades.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>Erstellen Sie den BizTalk Trading Partner Management (TPM) API Apps

> [AZURE.NOTE] Eine Instanz BizTalk Trading Partner Management (TPM) ist erforderlich, um die AS2-Connector und X12 oder EDIFACT-API App erstellen. Wenn Sie die TPM-API App erstellen, erstellen Sie die TPM-Instanz aus.

Die folgenden Schritte erstellen die TPM-Instanz aus:

1. Wählen Sie im Portal Azure Startboard (Homepage) **Marketplace**. **Apps API** Listet alle vorhandenen API Apps und Verbinder. Sie können auch **Suchen** , für die bestimmte B2B-API Apps.
2. Wählen Sie **BizTalk Trading Partner Management**aus. Wählen Sie in der neuen Blade **Erstellen**aus. 
3. Geben Sie die Eigenschaften aus: 

    Eigenschaft | Beschreibung
--- | ---
Namen | Geben Sie einen beliebigen Namen für die TPM-Instanz aus. Beispielsweise können Sie sie *AccountsPayableTPM*nennen.
Paketeinstellungen | Geben Sie die ADO.NET **Datenbankverbindungszeichenfolge** zur Azure SQL-Datenbank, die Sie erstellt haben. <br/><br/>Wenn Sie die Verbindungszeichenfolge kopieren, wird das Kennwort nicht die Verbindungszeichenfolge hinzugefügt. Achten Sie darauf, das Kennwort in der Verbindungszeichenfolge.
App-Serviceplan | Listen der Zahlungsplan. Sie können ihn ändern, wenn Sie mehr oder weniger Ressourcen benötigen.
Preise Ebene | Schreibgeschützte Eigenschaft, die die Preise in der Kategorie innerhalb Ihres Abonnements Azure aufgelistet. 
Ressourcengruppe | Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden. Alle Apps-API und Verbinder für Ihre Logik Apps, Web Apps und Mobile-Apps müssen in derselben Ressourcengruppe sein. <br/><br/>[Ressource Entwurfsphase](../azure-resource-manager/resource-group-overview.md) wird diese Eigenschaft erläutert. 
Abonnement | Schreibgeschützte Eigenschaft, die Listen Ihres aktuellen Abonnements.
Speicherort | Die geographischen Standort, der Ihre Azure Service hostet. 
Fügen zu Startboard hinzu | Wählen Sie diese Option, um die B2B-API-App zu Ihrer Starboard (Homepage) hinzufügen.

4. Wählen Sie auf **Erstellen**. 

Nachdem die TPM-API APP (TPM Instanz) erstellt wurde, können Sie dann AS2-Connector und/oder den X12 oder EDIFACT-API Apps erstellen. 


#### <a name="create-the-as2-connector"></a>Erstellen von AS2-connector

1. Wählen Sie im Portal Azure Startboard (Homepage) **Marketplace**. **Apps API** Listet alle vorhandenen API Apps und Verbinder. Sie können auch **Suchen** , für die bestimmte B2B-API Apps.
2. Wählen Sie **AS2 Verbinder**aus. Wählen Sie in der neuen Blade **Erstellen**aus. 
3. Geben Sie die Eigenschaften aus: 

    Eigenschaft | Beschreibung
--- | ---
Namen | Geben Sie einen beliebigen Namen für den AS2 Verbinder aus. Beispielsweise können Sie sie *AS2Connector*nennen.
Paketeinstellungen | Geben Sie die Einstellungen für bestimmte zu dieser App-API wie den TPM Instanznamen ein. <br/><br/>Finden Sie unter [Hinzufügen von AS2 Paketeinstellungen](#AddAS2Conn) in diesem Thema für die einzelnen Eigenschaften ein. 
App-Serviceplan | Listen der Zahlungsplan. Sie können ihn ändern, wenn Sie mehr oder weniger Ressourcen benötigen.
Preise Ebene | Schreibgeschützte Eigenschaft, die die Preise in der Kategorie innerhalb Ihres Abonnements Azure aufgelistet. 
Ressourcengruppe | Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden. [Ressource Entwurfsphase](../azure-resource-manager/resource-group-overview.md) wird diese Eigenschaft erläutert. 
Abonnement | Schreibgeschützte Eigenschaft, die Listen Ihres aktuellen Abonnements.
Speicherort | Die geographischen Standort, der Ihre Azure Service hostet. 
Fügen zu Startboard hinzu | Wählen Sie diese Option, um die B2B-API-App zu Ihrer Starboard (Homepage) hinzufügen.

    **<a name="AddAS2Conn"></a>AS2 Verbinder Paketeinstellungen**

    Eigenschaft | Beschreibung
--- | --- 
Datenbank-Verbindungszeichenfolge | Geben Sie die Verbindungszeichenfolge ADO.NET zur Azure SQL-Datenbank, die Sie erstellt haben. Wenn Sie die Verbindungszeichenfolge kopieren, wird das Kennwort nicht die Verbindungszeichenfolge hinzugefügt. Achten Sie darauf, geben das Kennwort in der Verbindungszeichenfolge zurück, bevor Sie einfügen.
Aktivieren der Archivierung für eingehende Nachrichten | Optional. Aktivieren Sie diese Eigenschaft, die Nachrichteneigenschaften einer eingehenden AS2-Nachricht erhalten von einem Partner zu speichern. 
Verbindungszeichenfolge für Azure Blob-Speicher  | Geben Sie die Verbindungszeichenfolge zum Container Azure BLOB-Speicher, die, den Sie erstellt haben. Wenn Archivierung aktiviert ist, werden die Nachrichten codierten und decodierten in diesem Speichercontainer gespeichert.
TPM Instanznamen | Geben Sie den Namen der **BizTalk Trading Partner Management** API App Sie zuvor erstellt haben. Beim Erstellen des Verbinders AS2 führt diesen Connector nur die AS2 Vereinbarungen innerhalb dieses bestimmten TPM-Instanz aus.

4. Wählen Sie auf **Erstellen**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>Erstellen der X12 oder EDIFACT API-Apps

1. Wählen Sie im Portal Azure Startboard (Homepage) **Marketplace**. **Apps API** Listet alle vorhandenen API Apps und Verbinder. Sie können auch **Suchen** , für die bestimmte B2B-API Apps.
2. Wählen Sie **BizTalk X 12** oder **EDIFACT BizTalk**. Wählen Sie in der neuen Blade **Erstellen**aus. 
3. Geben Sie die Eigenschaften aus: 

    Eigenschaft | Beschreibung
--- | ---
Namen | Geben Sie einen beliebigen Namen für die B2B-API App aus. Beispielsweise können Sie sie *EDI850APIApp*nennen.
Paketeinstellungen | Geben Sie die Einstellungen für bestimmte zu dieser App-API wie den TPM Instanznamen ein. <br/><br/>In diesem Thema für die einzelnen Eigenschaften finden Sie unter [X12 oder EDIFACT Paketeinstellungen](#AddX12) . 
App-Serviceplan | Listen der Zahlungsplan. Sie können ihn ändern, wenn Sie mehr oder weniger Ressourcen benötigen.
Preise Ebene | Schreibgeschützte Eigenschaft, die die Preise in der Kategorie innerhalb Ihres Abonnements Azure aufgelistet. 
Ressourcengruppe | Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden. [Ressource Entwurfsphase](../azure-resource-manager/resource-group-overview.md) wird diese Eigenschaft erläutert. 
Abonnement | Schreibgeschützte Eigenschaft, die Listen Ihres aktuellen Abonnements.
Speicherort | Die geographischen Standort, der Ihre Azure Service hostet. 
Fügen zu Startboard hinzu | Wählen Sie diese Option, um die B2B-API-App zu Ihrer Starboard (Homepage) hinzufügen.

    **<a name="AddX12"></a>X12 und EDIFACT API Apps Paketeinstellungen**  

    Eigenschaft | Beschreibung
--- | --- 
Datenbank-Verbindungszeichenfolge | Geben Sie die Verbindungszeichenfolge ADO.NET zur Azure SQL-Datenbank, die Sie erstellt haben. Wenn Sie die Verbindungszeichenfolge kopieren, wird das Kennwort nicht die Verbindungszeichenfolge hinzugefügt. Achten Sie darauf, geben das Kennwort in der Verbindungszeichenfolge zurück, bevor Sie einfügen.
Service Bus Namespace | Geben Sie den Dienstbus Namespace, die, den Sie erstellt haben. Erforderlich nur beim Batchverarbeitung aktiviert ist. 
Name der Dienst Bus Namespace freigegebene Access-Taste | Geben Sie den Namespace Dienstbus Zugriffstaste, die Sie erstellt haben. Erforderlich nur beim Batchverarbeitung aktiviert ist. 
Service Bus Namespace freigegebene Access Schlüssel Wert | Geben Sie den Namespace Dienstbus Zugriffstaste-Wert, den Sie erstellt haben. Erforderlich nur beim Batchverarbeitung aktiviert ist. 
TPM Instanznamen | Geben Sie den Namen der **BizTalk Trading Partner Management** API App Sie zuvor erstellt haben. Wenn Sie die X12 oder EDIFACT-API Apps erstellen, führt diese App API nur die X12/EDFIACT Vereinbarungen in die jeweilige Instanz TPM aus.

4. Wählen Sie auf **Erstellen**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Fügen Sie Ihrem Partner, Vereinbarungen, Zertifikate und schemas 
Öffnen Sie im Portal Azure Ihre TPM API App aus. Fügen Sie im Abschnitt **Komponenten** Ihrer Partner, Vereinbarungen, Zertifikate und Schemas aus. 

Sie können auch Vereinbarungen hinzufügen, Ihre AS2 Verbindern, X12 API Apps und EDIFACT API Apps. 


## <a name="monitor-your-api-apps"></a>Überwachen Sie Ihrer Apps API
Öffnen Sie im Portal Azure Ihre TPM API App aus. Im Abschnitt **Vorgänge** können Sie verschiedene Management Vorgänge anzeigen. Beispielsweise können Sie:

- Ansicht Information und Fehlermeldungen Ereignisse
- Anzeigen von Arbeitsspeicher Verwendung und Thread Anzahl der Worker-Prozess (w3wp)
- Die Anwendung und Web Serverprotokolle anzeigen

Mehr auf dem [Monitor Ihrer Apps Logik](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Hinzufügen der Apps-API an Ihrer Anwendung 
Microsoft Azure-App-Verwaltungsdienst macht Anwendungstypen von anderen, die diese B2B-API Apps verwenden können. Sie können erstellen Sie ein neues oder Ihrer vorhandenen B2B-API Apps Logik Apps, Mobile-Apps oder eine Web Apps hinzufügen. 

Innerhalb der App hinzugefügt Auswahl einfach die B2B-API Apps aus dem Katalog automatisch Ihre App.  

> [AZURE.IMPORTANT] Zum Hinzufügen von Verbindern und API-Apps, die Sie zuvor erstellt haben, erstellen Sie die Logik Apps, Mobile-Apps oder Web Apps in derselben Ressourcengruppe ein. 

Die folgenden Schritte durch Hinzufügen der B2B-API Apps Logik Apps, Mobile-Apps oder Web Apps: 

1. Azure-Portal Startboard (Homepage) wechseln Sie zu der **Marketplace**, und suchen Sie nach der Logik, Mobile oder Web Apps. 

    Wenn Sie eine neue App erstellen, suchen Sie nach Apps Logik, Mobile-Apps oder Web Apps. Wählen Sie die App aus, und wählen Sie in der neuen Blade **Erstellen**. [Erstellen einer App Logik](app-service-logic-create-a-logic-app.md) sind die Schritte aufgeführt. 

2. Öffnen Sie Ihre App, und wählen Sie **Trigger und Aktionen**. 

3. Wählen Sie aus dem **Katalog**die B2B-API seiner automatisch zu Ihrer Anwendung fügt hinzu. Sie können auch eine neue B2B-API App erstellen.

    > [AZURE.IMPORTANT] AS2-Connector und X12, erfordern EDIFACT-API Apps eine TPM-Instanz aus. Falls Sie erstellen neue B2B-API Apps, erstellen zuerst die TPM-API App, und erstellen Sie dann den AS2 Verbinder, X12 API App oder EDIFACT-API App. 

4. Wählen Sie **OK** , um die Änderungen zu speichern. 

>[AZURE.NOTE] Erste Schritte mit Azure Logik Apps vor dem Anmelden für ein Azure-Konto, [Versuchen Sie es Logik Apps](https://tryappservice.azure.com/?appservice=logic)haben. Sie können sofort eine kurzlebige Starter Logik app erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="more-b2b-resources"></a>Weitere B2B-Ressourcen

[Erstellen eines Prozesses B2B](app-service-logic-create-a-b2b-process.md)<br/>
[Erstellen eine separate Partner Vereinbarung treffen](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Was sind Verbindern und BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Erfahren Sie mehr über Logik Apps und Web Apps
[Was sind Logik Apps aus?](app-service-logic-what-are-logic-apps.md)<br/>
[Websites und Web Apps in Azure App-Verwaltungsdienst](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Weitere Verbinder

[Verbinder und API Apps-Liste](app-service-logic-connectors-list.md)<br/><br/>
[Was sind Verbindern und BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md) 
