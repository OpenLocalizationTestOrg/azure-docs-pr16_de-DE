<properties 
    pageTitle="Name des Herausgebers und Herausgeber Schlüssel BizTalk-Dienste | Microsoft Azure" 
    description="Informationen Sie zum Abrufen von Herausgeber Namen und Herausgeber Schlüssel für Dienstbus oder Access Control (ACS) BizTalk-Dienste. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
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




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel

Azure BizTalk-Dienste verwenden den Dienst Bus Herausgeber Namen und Herausgeber Key Access Steuerelement Herausgeber Name und Herausgeber-Taste. Insbesondere:

Aufgabe | Welche Herausgeber Namen und Herausgeber Schlüssel
--- | ---
Bereitstellen der Anwendung von Visual Studio | Zugriff auf Herausgeber Steuerelementname und Herausgeber Schlüssel
Konfigurieren des Azure BizTalk Services-Portals | Zugriff auf Herausgeber Steuerelementname und Herausgeber Schlüssel
Erstellen von LOB Relays mit BizTalk Netzwerkadapter-Dienste in Visual Studio | Dienst-Bus Herausgeber und Herausgeber Schlüssel

In diesem Thema werden die Schritte zum Abrufen der Herausgeber Name und Herausgeber Schlüssel aufgelistet. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Zugriff auf Herausgeber Steuerelementname und Herausgeber Schlüssel
Die Access-Steuerelementname Herausgeber und Herausgeber Schlüssel werden wie folgt verwendet:

- Ihre Azure BizTalk Service-Anwendung, die in Visual Studio erstellten: um Azure erfolgreich Ihrer BizTalk-Service-Anwendung in Visual Studio bereitstellen, geben Sie die Access-Steuerelementname Herausgeber und Herausgeber-Taste. 
- Das Azure BizTalk-Portal: Wenn Sie einen BizTalk-Service erstellen und das BizTalk-Portal zu öffnen, werden Ihre Access-Steuerelementname Herausgeber und Herausgeber Schlüssel automatisch für die Bereitstellung mit den gleichen Access Control Werten registriert.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopieren und Einfügen der Access-Steuerelementname Herausgeber und Herausgeber Schlüssel

1. Melden Sie sich mit dem [Azure klassische Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Wählen Sie im linken Navigationsbereich **BizTalk-Dienste**.
3. Wählen Sie den BizTalk-Dienst. 
4. Wählen Sie die **Verbindungsinformationen** in der Taskleiste. Die Access-Steuerelement-Namespace Standard Herausgeber (Herausgeber Name) und Standard-Taste (Herausgeber Schlüssel) aufgelistet sind, und können kopiert und eingefügt werden.  

Zusammenfassung:  
Namen des Herausgebers = Standard Herausgeber  
Herausgeber Key = Standard-Taste


Sie können auch **Öffnen ACS-Verwaltungsportal** die Steuerung des Benutzerzugriffs Werte auswählen:

1. Melden Sie sich mit dem [Azure klassische Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Wählen Sie im linken Navigationsbereich **BizTalk-Dienste**.
3. Wählen Sie den BizTalk-Dienst.
4. Wählen Sie die Verbindungsinformationen-Schaltfläche, und wählen Sie **Öffnen ACS-Verwaltungsportal**.
5. Wählen Sie im Portal unter **Diensteinstellungen** **Dienstidentitäten**aus. Dadurch werden Ihre Identität Dienst, der Ihre Access Herausgeber Steuerelementname Wert ist. Wählen Sie aus Ihrem Dienstidentität finden Sie unter das Kennwort ein, die einen Wert des Herausgebers Schlüssel ist. Ihre Werte können kopiert werden.<br/><br/>
**Dienstidentitäten**sehen Sie beispielsweise "Besitzer". "Besitzer" ist Ihre Access-Steuerelementname Herausgeber. Wenn Sie den Link "Besitzer" klicken, wird das **Kennwort ein**. Wenn Sie den Link "Kennwort" klicken, wird den Wert. Dieser Wert Kennwort ist Ihre Access-Taste Herausgeber.  

Zusammenfassung:  
Namen des Herausgebers = Dienstidentität Name  
Herausgeber Key = Kennwortwert

Klicken Sie im linken Navigationsbereich können Sie auch **Active Directory** zum Abrufen der Werte für die Access-Steuerelement auswählen. 

> [AZURE.IMPORTANT]Beim Erstellen einer Access-Steuerelement Namespace wird mit **Active Directory**, eine Identität Dienst **nicht** automatisch erstellt. Wenn Sie eine BizTalk Service, eine Access-Steuerelement-Namespace bereitstellen Dienstidentität mit dem Namen "Besitzer" (Name des Herausgebers) und Kennwort (Herausgeber Schlüssel), und symmetrischen Schlüssel werden automatisch erstellt.<br /> 
[How to: verwenden ACS-Verwaltungsdienst auf Dienstidentitäten konfigurieren](http://go.microsoft.com/fwlink/p/?LinkID=303942) enthält weitere Informationen zu Access Control Service Identitäten.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Dienst-Bus Herausgeber und Herausgeber Schlüssel
Service Bus Herausgeber Name und Herausgeber Schlüssel werden von BizTalk Netzwerkadapter-Dienste verwendet. Im Projekt BizTalk-Dienste in Visual Studio verwenden Sie die BizTalk-Dienste Netzwerkadapter Verbindung zu einem lokalen Linie-von-branchenspezifische System. Um eine Verbindung herzustellen, die LOB-Relay erstellen, und geben Sie die Details Ihrer LOB-System. Wenn Sie diese Aufgabe ausführen, geben Sie auch die Dienst-Bus Herausgeber und Herausgeber-Taste.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Zum Abrufen der Dienst-Bus Herausgeber und Herausgeber Schlüssel

1. Melden Sie sich mit dem [Azure klassische Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Wählen Sie im linken Navigationsbereich **Dienstbus**aus.
3. Wählen Sie Ihren Namespace aus. Wählen Sie in der Taskleiste **Verbindungsinformationen**ein. Dies zeigt die **Herausgeber Standard** (Herausgeber Name) und die **Taste Standard** (Herausgeber-Taste). Ihre Werte können kopiert werden.  

Zusammenfassung:  
Namen des Herausgebers = Standard Herausgeber  
Herausgeber Key = Standard-Taste

## <a name="next"></a>Weiter
Weitere Themen im Azure BizTalk-Dienste:

-  [Installieren die Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Lernprogramme: Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Siehe auch
-  [How to: Verwenden der ACS-Verwaltungsdienst Dienstidentitäten konfigurieren](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium-Editionen Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Dienste: Provisioning klassischen mithilfe von Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Dienste: Provisioning Statusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Dienste: Sichern und Wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Dienste: Begrenzungsebene](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
