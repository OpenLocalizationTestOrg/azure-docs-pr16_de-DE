<properties
    pageTitle="Freigeben von Notizen für Azure BizTalk-Dienste | Microsoft Azure BizTalk-Dienste"
    description="Listen der bekannte Probleme bei Azure BizTalk-Dienste" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Versionsinformationen für Azure BizTalk-Dienste

Die Versionsinformationen für die BizTalk-Dienste von Microsoft Azure enthalten die bekannte Probleme in dieser Version.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Was ist neu in der November Aktualisieren der BizTalk-Dienste
* Verschlüsselung bei Rest kann im Portal BizTalk aktiviert sein. [Aktivieren Sie die Verschlüsselung bei Rest BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn874052.aspx)finden Sie unter.

## <a name="update-history"></a>Updateverlauf

### <a name="october-update"></a>Oktober aktualisieren

* Organisationskonten werden unterstützt:  
 * **Szenario**: Sie eine BizTalk Service-Bereitstellung mit einem Microsoft-Konto registriert (wie user@live.com). In diesem Szenario können nur Microsoft Account Benutzer der BizTalk Service mit dem Portal BizTalk-Dienste verwalten. Ein Organisations-Konto kann nicht verwendet werden.  
 * **Szenario**: Sie registriert eine BizTalk Service-Bereitstellung mit einem Unternehmenskonto in einem Azure Active Directory (wie user@fabrikam.com oder user@contoso.com). In diesem Szenario können nur Azure Active Directory-Benutzer in der gleichen Organisation der BizTalk Service mit dem Portal BizTalk-Dienste verwalten. Ein Microsoft-Konto kann nicht verwendet werden.  
* Wenn Sie einen BizTalk-Service im klassischen Azure-Portal erstellen, werden Sie automatisch im Portal BizTalk registriert.
 * **Szenario**: Erstellen einer BizTalk Service Sie melden Sie sich bei der Azure klassischen-Portal, und wählen Sie **Verwalten** zum ersten Mal. Wenn das BizTalk-Portal geöffnet wird, wird der BizTalk Service automatisch registriert und ist für die Bereitstellung Ihrer bereit.  
 Finden Sie unter [registrieren und Aktualisieren einer-Bereitstellung BizTalk-Dienst, klicken Sie auf das Portal BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>August 14 aktualisieren
* Vertrag und einen Controller Entkopplung – Trading Partner Agreements und Brücken sind jetzt im Portal BizTalk abgekoppelt. Sie jetzt erstellen Agreements und Brücken separat und zur Laufzeit Brücken auflösen, um eine Vereinbarung basierend auf den Werten in der EDI-Nachricht. Finden Sie unter [Erstellen von Vereinbarungen Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689908.aspx), [eine EDI Brücke mit BizTalk Services-Portal erstellen](https://msdn.microsoft.com/library/azure/dn793986.aspx), [erstellen eine AS2 Brücke BizTalk Services-Portal verwenden](https://msdn.microsoft.com/library/azure/dn793993.aspx), und [wie behebe Brücken Agreements zur Laufzeit?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Die Option zum Erstellen von Vorlagen für Kaufverträge () ist nicht mehr vorhanden sind.  
* Für die Vereinbarung angeordneten senden können Sie jetzt verschiedenen Trennzeichen Mengen für jedes Schema angeben. Klicken Sie unter Protokoll Einstellungen für die Seite Lizenzvertrag Senden dieser Konfiguration angegeben. Weitere Informationen finden Sie unter [erstellen ein X12 Vertrag Azure BizTalk Dienste](https://msdn.microsoft.com/library/azure/hh689847.aspx) und [Erstellen eines EDIFACT Vertrag Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/dn606267.aspx). Zwei neue Elemente werden auch die TPM OM-API für den gleichen Zweck hinzugefügt. Finden Sie unter [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) und [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standard XSD-Konstrukte, einschließlich Typen abgeleitet werden jetzt unterstützt. Finden Sie unter [verwenden standard XSD Ihre Karten erstellt](https://msdn.microsoft.com/library/azure/dn793987.aspx) und [Abgeleitet Benutzertypen in Zuordnung Szenarien und Beispiele](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 unterstützt das neue Mikrofon-Algorithmen zum Signieren von Nachrichten und neue Algorithmen für die Verschlüsselung. Finden Sie unter [erstellen einen AS2 Vertrag Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Bekannte Probleme

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Verbindungsprobleme nach BizTalk Portal Update Services

  Wenn Sie über das Portal BizTalk BizTalk Services aktualisiert wird, um Änderungen an den Dienst Rollen bei geöffneter verfügen, möglicherweise Verbindungsprobleme mit das BizTalk-Portal bestehen.  
  Problem zu umgehen können Sie den Browser neu starten, Löschen des Browser-Cache oder im Portal im privaten Modus starten.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio-IDE kann nicht das Element gefunden werden, wenn Sie auf einen Fehler oder eine Warnung in einem Projekt BizTalk-Dienste
Installieren der Visual Studio 2012 Update 3 RC 1 um das Problem zu beheben.  

### <a name="custom-binding-project-reference"></a>Benutzerdefinierte Bindung Project Bezug
Berücksichtigen Sie die folgenden Situationen mit einem Projekt BizTalk-Dienste in Visual Studio-Lösung:  
* In der gleichen Visual Studio-Lösung ist es ein BizTalk Services-Projekt und ein benutzerdefinierter Bindung Projekt. Das Projekt BizTalk Service weist einen Verweis auf diese benutzerdefinierte Bindung Projektdatei.
* Das Projekt BizTalk Service hat einen Verweis auf eine benutzerdefinierte Bindung/Verhalten DLL.

Sie erstellen' ' der Lösung in Visual Studio erfolgreich. Klicken Sie dann 'Neu' oder 'Bereinigen' die Lösung. Wenn Sie neu erstellen oder erneut bereinigen anschließend tritt der folgende Fehler:  
  Datei konnte nicht kopiert <Path to DLL> zu "bin\Debug\FileName.dll". Der Prozess kann nicht die Datei 'bin\Debug\FileName.dll' zugreifen, da sie von einem anderen Prozess verwendet wird.  

#### <a name="workaround"></a>Dieses Problem zu umgehen
* Wenn [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) installiert ist, müssen Sie die folgenden beiden Optionen aus:

  * Starten Sie Visual Studio oder

  * Starten Sie die Lösung neu. Führen Sie dann nur eine erstellen, klicken Sie auf die Lösung.  

* Wenn [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) nicht installiert ist, öffnen Sie Task-Manager zu, klicken Sie auf der Registerkarte Prozesse, klicken Sie auf die MSBuild.exe-Prozess, und klicken Sie dann auf die Schaltfläche Prozess beenden.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Weiterleitung an BasicHttpRelay Endpunkte wird nicht von Brücken und BizTalk Services Portal unterstützt, wenn nicht druckbare Zeichen als HTTP-Header höhergestuft wurden

Wenn Sie nicht druckbare Zeichen als Teil der Höhergestufte Eigenschaften für Nachrichten verwenden, können nicht die Nachrichten an Relay Ziele weitergeleitet werden, die die BasicHttpRelay Bindung verwenden. Darüber hinaus stehen die heraufgestuften Eigenschaften, die als Verlauf URL-codierte für Blobs und dauerhaften encoded für Ziele gehören.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN asynchrone gesendet werden, auch wenn die Option senden asynchrone MDN nicht aktiviert ist.  
Stellen Sie dieses Szenario – aus, wenn Sie aktivieren Sie das Kontrollkästchen **asynchrone MDN senden** , geben Sie einen URL zum Senden der asynchronen MDN zu, und deaktivieren Sie das Kontrollkästchen **Senden asynchrone MDN** erneut, die MDN wird weiterhin gesendet zur angegebenen URL, obwohl die Option zum Senden von asynchronen MDNs nicht aktiviert ist.  
Problem zu umgehen, müssen die angegebene URL gelöscht werden, bevor die **asynchrone MDN senden** das Kontrollkästchen deaktivieren und dann den Vertrag AS2 bereitstellen.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Leerzeichen hinter einem gültigen Interchange dazu führen, dass eine leere Nachricht an den Endpunkt unterbrechen gesendet werden  
Wenn es Leerzeichen hinter einem Segment "IEA", der Disassembler behandelt dies als Ende des aktuellen Interchange und befasst sich mit den nächsten Satz von Leerzeichen als nächsten Nachricht. Da dies nicht gültig Interchange ist, können Sie feststellen, dass eine Meldung zur erfolgreichen zum Routing Ziel gesendet wird, und eine leere den Endpunkt unterbrechen Nachricht.  
### <a name="tracking-in-biztalk-services-portal"></a>Überwachung BizTalk Services-Portal  
Verlauf Ereignisse sind auf die Verarbeitung von EDI-Nachrichten und alle Korrelationskoeffizienten erfasst. Wenn eine Nachricht außerhalb der Phase Protokoll fehlschlägt, wird der Verlauf als erfolgreich angezeigt. Finden Sie in diesem Fall unter der Spalte **Details** **Verfolgen von** Fehlerdetails im Abschnitt Protokoll.
Die X12 Einstellungen senden und empfangen ([erstellen ein X12 Vertrag Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689847.aspx)) finden Sie Informationen zu der Phase Protokoll.  

### <a name="update-agreement"></a>Update-Vertrag  
Das BizTalk-Portal ermöglicht es Ihnen, die Kennung einer Identität zu ändern, wenn eine Vereinbarung konfiguriert ist. Dies kann dazu führen, dass Eigenschaften stimmt nicht Attributlänge überein. Es gibt beispielsweise eine Vereinbarung mit ZZ:1234567 und ZZ:7654321 die Kennung. Ändern Sie die einräumen BizTalk Services-Portal ZZ:1234567 01:ChangedValue benutzerspezifisch. Öffnen Sie den Vertrag und 01:ChangedValue anstelle von ZZ:1234567 angezeigt wird.
Um die Kennung einer Identität zu ändern, löschen Sie den Vertrag, **Identitäten** in dieses Profil aktualisieren, und erstellen Sie dann den Vertrag erneut.  
> AZURE. Warnung dieses Verhalten wirkt sich auf X12 und AS2.  

### <a name="as2-attachments"></a>AS2 Anlagen  
Anlagen für AS2 Nachrichten nicht unterstützt werden, senden oder empfangen. Insbesondere Anlagen werden im Hintergrund ignoriert und der Nachrichtentext wird als eine normale AS2 Nachricht verarbeitet.  
### <a name="resources-remembering-path"></a>Ressourcen: Erinnern Pfad an.  
Klicken Sie im Dialogfeld möglicherweise den Pfad zuvor zum Hinzufügen einer Ressource nicht beachten Sie beim Hinzufügen von **Ressourcen**. Um den zuvor verwendeten Pfad Denken Sie daran, versuchen Sie, die BizTalk Services Portal-Website auf **Vertrauenswürdige Sites** in Internet Explorer hinzufügen.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Wenn Sie benennen Sie eine Verbindung die Entität, und schließen Sie das Projekt zu speichern, führt die Entität erneut öffnen, zu einem Fehler
Stellen Sie eine Szenario in der folgenden Reihenfolge aus:  
* Fügen Sie eine Verbindung (beispielsweise eine XML-unidirektionalen Bridge) zu einem Projekt BizTalk Service  

* Benennen Sie die Brücke durch Angeben eines Werts für die Eigenschaft Entitätsname ein. Dadurch wird die zugeordneten .bridgeconfig-Datei mit dem Namen für die angegebene umbenannt.  

* Schließen Sie die Datei .bcs (nach Schließen der Registerkarte in Visual Studio), ohne die Änderungen zu speichern.  

* Öffnen Sie die Datei .bcs erneut aus der Lösung Explorer aus.  
Sie können feststellen, dass während die Datei zugeordnete .bridgeconfig den neuen Namen, die, den Sie angegeben haben enthält, der Namen der Person auf der Entwurfsoberfläche immer noch den alten Namen. Wenn Sie versuchen, die Brückenkonfiguration Doppelklicken Sie auf die Komponente Bridge öffnen, erhalten Sie folgenden Fehler auf:  
  '<old name>'Entität zugeordnete Datei'<old name>.bridgeconfig' ist nicht vorhanden  
Um die Ausführung in diesem Szenario zu vermeiden, sicherzustellen Sie, dass die Änderungen zu speichern, nachdem Sie die Elemente in einem Projekt BizTalk Service umbenennen.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk Service-Projekt wird auch wenn ein Element aus einem Visual Studio-Projekt ausgeschlossen wurde erfolgreich erstellt
Erwägen Sie ein Szenario, in dem Sie ein Element (beispielsweise eine XSD-Datei) hinzufügen, zu einem Projekt BizTalk Service, beziehen Sie dieses Element in der Brückenkonfiguration (z. B., indem sie als Anforderung Nachrichtentyp festzulegen), und schließen Sie es dann mit dem Visual Studio-Projekt aus. In diesem Fall erhalten Erstellen des Projekts keine Fehler, solange das gelöschte Element steht auf dem Datenträger am selben Speicherort aus, in dem sie in der Visual Studio-Projekt eingeschlossen wurde.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Das Projekt BizTalk Service überprüft für Schema Verfügbarkeit nicht beim Konfigurieren der Brücken
In einem Projekt BizTalk Service Wenn ein Schema, das dem Projekt hinzugefügt wird, ein anderes Schema importiert überprüft BizTalk Service Projekt nicht, ob das importierte Schema zum Projekt hinzugefügt wurde. Wenn Sie versuchen, eine solche ein Projekt erstellen, erhalten Sie nicht Fehlern erstellen.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Die Antwortnachricht für eine XML-Anforderung-Antwort-Verbindung ist immer der Zeichensatz UTF-8
In dieser Version wird der Zeichensatz der Antwort aus einer XML-Anforderung-Antwort Bridge immer auf UTF-8 festgelegt.
### <a name="user-defined-datatypes"></a>Benutzerdefinierte Datentypen
Benutzerdefinierte Datentypen für Netzwerkadapter Vorgänge können die BizTalk Netzwerkadapter Pack Netzwerkadapter innerhalb der Funktion BizTalk Dienst genutzt werden.
Wenn Sie benutzerdefinierte Datentypen verwenden möchten, kopieren Sie die Dateien (DLL), zu Laufwerk: \Programme\Microsoft BizTalk Netzwerkadapter Service\BAServiceRuntime\bin\ oder in der globalen Cache Assemblycache () auf dem Server den Dienst BizTalk Hostingdienst. Andernfalls möglicherweise der folgende Fehler auf dem Client auftreten:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Es wird empfohlen, um GACUtil.exe verwenden, um eine Datei im globalen Assemblycache zu installieren. GACUtil.exe Dokumente wie dieses Tool und die Befehlszeilenoptionen für Visual Studio verwendet.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Einen Neustart von der BizTalk Netzwerkadapter Service-Website
Installieren der **BizTalk Netzwerkadapter-Dienstlaufzeit** * erstellt die * *Dienst BizTalk* * Website in IIS, die enthält die * *BAService* * Anwendung.* *BAService** Anwendung intern Relay Bindung verwendet, um die Reichweite von lokal-Endpunkt in der Cloud zu erweitern. Für einen Dienst gehostet lokal wird der entsprechende Relay Endpunkt am Dienstbus registriert nur, wenn der lokale Dienst gestartet wird.  

Wenn Sie beenden, und starten die Anwendung, wird die Konfiguration für eine Anwendung automatisch startet nicht berücksichtigt. Daher müssen beim **BAService** beendet wird, müssen Sie immer der **BizTalk Netzwerkadapter Service** -Website stattdessen neu starten. Beginnen Sie oder beenden Sie die Anwendung **BAService** nicht.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Sonderzeichen sollte für Namen von LOB Komponenten Adresse und Entität nicht verwendet werden
Sie sollten nicht für Namen von LOB Komponenten Adresse und Entität Sonderzeichen verwenden. Wenn Sie dies tun, erhalten Sie einen Fehler, beim Bereitstellen des Projekts BizTalk Service. Für bestimmte Zeichen wie "%" die Website BizTalk Dienst möglicherweise wechseln Sie in einem Zustand beendet und, müssen Sie ihn manuell starten.
### <a name="test-map-with-get-context-property"></a>Testen der Karte mit Get-Kontext-Eigenschaft
Wenn eine Transformation einen **Kontexteigenschaft erste** Karte Vorgang enthält, tritt ein Fehler **Zuordnung testen** . Ersetzen Sie den **Kontexteigenschaft erste** Karte Vorgang als vorübergehende Lösung mit einer Zeichenfolge zu verketten Karte Operation,-platzhalterprodukt Daten enthalten. Das Zielschema zu füllen wird und zulassen, dass Sie andere Transformation Funktionalität zu testen.
### <a name="test-map-property-does-not-display"></a>Testen der Karte Eigenschaft wird nicht angezeigt.
Die Eigenschaften für die **Zuordnung testen** können nicht in Visual Studio anzeigen. Dies kann auftreten, wenn **das Eigenschaftenfenster** und **Solution Explorer** -Fenster nicht gleichzeitig angedockt sind. Um dieses Verhalten zu beheben, docken Sie die **Eigenschaften** und der **Lösung Explorer** -Fenster an.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>DateTime zu formatierenden Dropdown-Liste ist abgeblendet.
Wenn eine DateTime formatierenden Map-Vorgang auf die Entwurfsoberfläche hinzugefügt und so konfiguriert, möglicherweise die Dropdown-Liste Format abgeblendet. Dies kann geschehen, wenn der Computer Anzeige **Mittel-125 %** oder **größer – 150 Prozent**festgelegt ist. Zur Behebung stellen Sie die Anzeige zu **kleiner – 100 % (Standard)** verwenden die folgenden Schritte aus:  
1. Öffnen Sie die **Systemsteuerung** , und klicken Sie auf **Darstellung und Anpassung**.
2. Klicken Sie auf **Anzeigen**.
3. Klicken Sie auf **kleiner – 100 % (Standard)** , und klicken Sie auf **Übernehmen**.

Die Dropdown-Liste **Format** sollte nun wie erwartet funktionieren.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Doppelte Vereinbarungen im BizTalk-Portal
Erwägen Sie das folgende Szenario:
1. Erstellen Sie einen Vertrag mithilfe der Trading Partner Management OM-API.
2. Öffnen Sie den Vertrag im Portal BizTalk in zwei verschiedenen Registerkarten ein.
3. Bereitstellen der Vereinbarung aus beiden Registerkarten an.
4. Daher abrufen sowohl die Verträgen bereitgestellt in doppelte Einträge im Portal BizTalk resultierender

**Dieses Problem zu umgehen**. Öffnen Sie einen der doppelten Verträge im Portal BizTalk und zurücknehmen.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Brücken verwenden aktualisiertes Zertifikat Sie nicht auch nach ein Zertifikats im Speicher Element aktualisiert wurde
Berücksichtigen Sie die folgenden Szenarien:  

**Szenario 1: Fingerabdruck-basierte Zertifikate bei der Sicherung von Nachrichtenübermittlung aus eine Verbindung zu einem Dienstendpunkt mit**  
Erwägen Sie ein Szenario, in dem Sie Fingerabdruck-basierte Zertifikate in Ihrem Projekt BizTalk Service verwenden. Sie Aktualisieren des Zertifikats im BizTalk-Portal mit demselben Namen, aber einen anderen Fingerabdruck, aber das Projekt BizTalk Service nicht entsprechend aktualisiert. In diesem Fall möglicherweise die Brücke fahren Sie mit die Nachrichten verarbeitet werden, da die älteren Zertifikatsdaten weiterhin im Cache Channel möglicherweise. Anschließend, schlägt die Verarbeitung von Nachrichten.  

**Dieses Problem zu umgehen**: Aktualisieren des Zertifikats im Projekt BizTalk Service und erneut bereitstellen des Projekts.  

**Szenario 2: Anhand eines Namens Verhalten verwenden, Identifizieren von Zertifikaten für Schutz der Nachrichtenübermittlung aus eine Verbindung zu einem Dienstendpunkt verwendet**

Erwägen Sie ein Szenario, in dem Sie Verhalten anhand eines Namens verwenden, Identifizieren von Zertifikaten im Projekt BizTalk Service verwendet. Aktualisieren des Zertifikats im Portal BizTalk, aber das Projekt BizTalk Service nicht entsprechend aktualisiert. In diesem Fall möglicherweise die Brücke fahren Sie mit die Nachrichten verarbeitet werden, da die älteren Zertifikatsdaten weiterhin im Cache Channel möglicherweise. Anschließend, schlägt die Verarbeitung von Nachrichten.  

**Dieses Problem zu umgehen**: Aktualisieren des Zertifikats im Projekt BizTalk Service und erneut bereitstellen des Projekts.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Brücken fortsetzen, um Nachrichten zu verarbeiten, auch wenn die SQL-Datenbank offline ist
Die BizTalk-Dienste Brücken weiterhin zum Verarbeiten von Nachrichten für eine Weile, auch wenn der Microsoft Azure SQL-Datenbank (der die laufende Informationen wie bereitgestellten Elemente und Pipelines gespeichert werden kann), ist offline. Dies liegt daran BizTalk-Dienste die zwischengespeicherten Elemente und Brückenkonfiguration verwendet.
Wenn Sie nicht möchten, führen Sie die Brücken keine Nachrichten verarbeitet werden soll, wenn der SQL-Datenbank offline ist, können Sie die BizTalk Services PowerShell-Cmdlets verwenden, beenden oder Anhalten der BizTalk Service. Finden Sie unter [Azure BizTalk Dienst für die Verwaltung](http://go.microsoft.com/fwlink/p/?LinkID=329019) der Windows PowerShell-Cmdlets zum Verwalten von Vorgängen.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Lesen die XML-Nachricht innerhalb des eine Verbindung benutzerdefiniertem Codekomponente enthält ein zusätzliches Stücklisten Zeichen
Erwägen Sie ein Szenario, in eine XML-Nachricht in eine Verbindung des benutzerdefinierten Code gelesen werden soll. Wenn Sie die .NET API System.Text.Encoding.UTF8.GetString(bytes) ein zusätzliches Stücklisten Zeichen in der Ausgabe am Anfang der Nachricht enthalten ist verwenden. Ja, wenn Sie nicht die Ausgabe an das zusätzliche Stücklisten Zeichen enthalten soll, müssen Sie verwenden ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Senden von Nachrichten an eine Verbindung mithilfe von WCF skalieren nicht
Um eine Verbindung mit WCF gesendete Nachrichten lässt sich nicht skalieren. Sie sollten stattdessen HttpWebRequest verwenden, wenn einen skalierbaren Client soll.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>Aktualisierung: Fehler bei Token Anbieter nach einem Upgrade von BizTalk Services Vorschau zu allgemeinen Verfügbarkeit (GA)
Es gibt eine EDI oder AS2 Vertrag mit aktiven Blattnamen ein. Wenn der BizTalk Service von Vorschau auf GA aktualisiert wird, können die folgenden auftreten:
* Fehler: Token-Provider konnte kein Sicherheitstoken bereitstellen. Token-Provider Nachricht zurückgegeben: der remote-Name nicht aufgelöst werden konnte.

* Stapelverarbeitungsaufgaben werden abgebrochen.

**Dieses Problem zu umgehen**: nach der BizTalk-Dienst wird aktualisiert, um allgemeine Verfügbarkeit (GA), erneut den Vertrag bereitstellen.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Toolbox zeigt die alte Bridge Symbole nach einem Upgrade von BizTalk-Dienste SDK
Nachdem Sie ein eine frühere Version von BizTalk-Dienste SDK, die alte Brücken als Symbole wurde Upgrade, weiterhin Toolbox die alte Symbole für die Brücken anzeigen. Wenn Sie eine Verbindung zur BizTalk Service Project-Designer-Oberfläche hinzufügen, zeigt die Fläche jedoch das neue Symbol.  

**Dieses Problem zu umgehen**. Sie können dieses Problem umgehen, durch Löschen der .tbd Dateien unter <system drive>: \Users\<Benutzer > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: BizTalk Portal Aktualisieren von der Preview GA möglicherweise eine Fehlermeldung angezeigt, die EDI-Funktion ist nicht verfügbar angezeigt
Wenn Sie das BizTalk-Portal angemeldet sind, während die BizTalk-Dienste von Vorschau auf GA aktualisiert wird, können Sie im Portal die folgende Fehlermeldung erhalten:  

Diese Funktion ist nicht verfügbar als Teil dieser Version von Microsoft Azure BizTalk-Dienste. Wechseln zum Verwenden dieser Funktionen auf eine geeignete Edition ein.  

**Lösung**: Abmelden aus dem Portal, schließen und öffnen im Browser, und klicken Sie dann melden Sie sich bei dem Portal.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Nachverfolgen von Daten neu wird nicht angezeigt, nach der Aktualisierung der BizTalk-Dienste auf GA
Angenommen Sie, ein Szenario, in dem Sie eine XML-Brücke BizTalk Services Preview-Abonnements bereitgestellt haben. Senden Sie Nachrichten zur Brücke und das entsprechende Nachverfolgen von Daten auf das Portal BizTalk verfügbar ist. Nun werden, wenn die Laufzeit Bits BizTalk Services-Portal und BizTalk-Dienste zu GA aktualisiert werden, und einer Nachricht an den gleichen Bridge Endpunkt einer früheren Version bereitgestellt senden, Verlauf Daten nicht für Nachrichten, die nach dem Upgrade gesendet angezeigt.  

### <a name="pipelines-vs-bridges"></a>Pipelines V/s Brücken
In diesem Dokument werden der Begriff 'Pipelines' und 'Brücken' synonym verwendet. Beide im Wesentlichen Bedeutung dieselbe, die eine Nachricht Processing Unit auf BizTalk-Dienste bereitgestellt wird.  

### <a name="concepts"></a>Konzepte  

[BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
