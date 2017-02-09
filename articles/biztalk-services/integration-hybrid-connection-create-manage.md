<properties 
    pageTitle="Erstellen und Verwalten von Verbindungen Hybrid | Microsoft Azure" 
    description="Informationen Sie zum Herstellen einer Verbindung Hybrid, die Verbindung zu verwalten und Installieren der Hybrid-Verbindungs-Manager. MABS, WABS" 
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
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Erstellen und Verwalten von Verbindungen Hybrid


## <a name="overview-of-the-steps"></a>Übersicht über die Schritte
1. Erstellen Sie eine Hybrid-Verbindung durch den **Hostnamen** oder **FQDN** der lokalen Ressource in Ihr privates Netzwerk eingeben.
2. Verknüpfen Sie Ihrer Azure Web apps oder Azure mobile-apps, die in Verbindung.
3. Installieren der Hybrid-Verbindungs-Manager für Ihre lokale Ressource, und Verbinden mit bestimmten Hybrid Verbindung. Azure-Portal bietet einzelner Klick zum Installieren und verbinden.
4. Verwalten Sie Hybrid-Verbindungen und ihre Verbindungsschlüssel.

Dieses Thema listet die folgenden Schritte aus. 

> [AZURE.IMPORTANT] Es ist möglich, einen Endpunkt Hybrid-Verbindung zu einer IP-Adresse festzulegen. Wenn Sie eine IP-Adresse verwenden, kann oder die lokale Ressource, je nach Ihren Kunden möglicherweise nicht erreicht haben. Die Verbindung Hybrid hängt von der Client eine DNS-Suche ausführen. In den meisten Fällen ist der __Client__ -Anwendungscode Ihrer. Wenn der Client eine DNS-Suche, nicht durchgeführt wird (es wird nicht versucht, die IP-Adresse zu beheben, als wäre sie einen Domänennamen (x.x.x.x)), und klicken Sie dann den Datenverkehr nicht über die Verbindung Hybrid gesendet wird.
>
> Beispiel (Pseudocode) definieren Sie **10.4.5.6** als lokalen Host aus:
> 
> **Das folgende Szenario funktioniert:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Das folgende Szenario funktioniert nicht:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="a-namecreatehybridconnectionacreate-a-hybrid-connection"></a><a name="CreateHybridConnection"></a>Erstellen einer Verbindung Hybrid

Eine Verbindung Hybrid können im Azure-Portal mit Web Apps **oder** mithilfe der BizTalk-Dienste erstellt werden. 

**Hybrid-Verbindungen mit Web Apps zu erstellen**, finden Sie unter [Verbinden von Azure Web Apps, die einer Ressource auf lokale](../app-service-web/web-sites-hybrid-connection-get-started.md). Sie können auch die Hybrid Verbindung Manager (HCM) aus der Web-app installieren, also die bevorzugte Methode. 

**Hybrid Verbindungen BizTalk-Dienste zu erstellen**:

1. Melden Sie sich mit dem [Azure klassische Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Klicken Sie im linken Navigationsbereich **BizTalk-Dienste** wählen Sie aus, und wählen Sie dann auf Ihre BizTalk Service. 

    Wenn Sie einen vorhandenen BizTalk Service besitzen, können Sie [Erstellen eines BizTalk-Dienstes](biztalk-provision-services.md)aus.
3. Wählen Sie die Registerkarte **Hybrid Verbindungen** aus:  
![Registerkarte Hybrid-Verbindungen][HybridConnectionTab]

4. Wählen Sie **in Verbindung erstellen** , oder wählen Sie die Schaltfläche **Hinzufügen** , in der Taskleiste. Geben Sie Folgendes ein:

    Eigenschaft | Beschreibung
--- | ---
Namen | Der Hybrid Verbindungsname muss eindeutig sein und darf nicht denselben Namen wie der BizTalk Service sein. Sie können einen beliebigen Namen geben jedoch Seien Sie spezifisch mit ihren Zweck. Beispiele für:<br/><br/>Gehaltsbuchhaltung*SQLServer*<br/>SupplyList*SharepointServer*<br/>Kunden*OracleServer*
Hostname | Geben Sie den vollqualifizierten Hostnamen, nur der Hostname oder die IPv4-Adresse der lokalen Ressource. Beispiele für:<br/><br/>mySQLServer<br/>*MySQLServer*. *Domäne*. corp.*YourCompany*.com<br/>*myHTTPSharePointServer*<br/>*MyHTTPSharePointServer*. .com *yourCompany*<br/>10.100.10.10<br/><br/>Wenn Sie die IPv4-Adresse verwenden, beachten Sie, dass Ihre Kunden oder einer Anwendung Code die IP-Adresse nicht behoben werden kann. Finden Sie unter der Wichtig Beachten Sie oben in diesem Thema.
Port | Geben Sie die Port-Nummer der lokalen Ressource ein. Wenn Sie Web Apps verwenden, geben Sie zum Beispiel Port 80 oder 443. Wenn Sie SQL Server verwenden, geben Sie Anschluss 1433.

5. Wählen Sie das Kontrollkästchen, um die Einrichtung abzuschließen. 

#### <a name="additional"></a>Zusätzliche

- Mehrere Hybrid-Verbindungen können erstellt werden. Finden Sie unter der [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md) für die Anzahl der Verbindungen zulässig. 
- Jede in Verbindung mit zwei Verbindungszeichenfolgen erstellt wird: Anwendung Tasten dieser senden und lokale Tasten, die das ÜBERWACHEN. Jedes Paar hat einen primären und sekundären-Taste. 


## <a name="a-namelinkwebsitealink-your-azure-app-service-web-app-or-mobile-app"></a><a name="LinkWebSite"></a>Verknüpfen Sie Ihrer Azure App-Verwaltungsdienst Web App oder Mobile-App

Wählen Sie Link einer Web App oder Mobile-App in Azure-App-Verwaltungsdienst zu einer vorhandenen Hybrid-Verbindung **Verwenden einer bestehenden Hybrid-Verbindung** in das Blade Hybrid Verbindungen aus. Finden Sie unter [Zugriff auf lokale Ressourcen Hybrid Verbindungen in Azure-App-Dienst verwenden](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="a-nameinstallhcmainstall-the-hybrid-connection-manager-on-premises"></a><a name="InstallHCM"></a>Installieren Sie die lokale Hybrid Verbindungs-Managers

Nach der Erstellung einer hybriden Verbindung Installieren der Hybrid-Verbindungs-Manager auf die lokale Ressource. Sie können aus Ihrer Azure Web apps oder in Ihrem BizTalk Service heruntergeladen werden. BizTalk-Dienste Schritte aus: 

1. Melden Sie sich zum [Azure klassischen Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Klicken Sie im linken Navigationsbereich **BizTalk-Dienste** wählen Sie aus, und wählen Sie dann auf Ihre BizTalk Service. 
3. Wählen Sie die Registerkarte **Hybrid Verbindungen** aus:  
![Registerkarte Hybrid-Verbindungen][HybridConnectionTab]
4. Wählen Sie in der Taskleiste aus **Lokalen einrichten**:  
![Lokale einrichten][HCOnPremSetup]
5. Wählen Sie **Installieren und konfigurieren** ausführen oder der Hybrid-Verbindungs-Manager auf dem lokalen System herunterladen. 
6. Wählen Sie das Häkchen, um die Installation zu starten. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Zusätzliche
- Hybrid-Verbindungs-Managers kann unter den folgenden Betriebssystemen installiert werden:

    - Windows Server 2008 R2 (.NET Framework 4.5 + und Windows Management Framework 4.0 + erforderlich)
    - Windows Server 2012 (Windows Management Framework 4.0 + erforderlich)
    - Windows Server 2012 R2


- Nach der Installation des Hybrid-Verbindungs-Managers, geschieht Folgendes: 

    - Die Hybrid Verbindung auf Azure gehostet wird automatisch konfiguriert, um die Verbindungszeichenfolge der primären Anwendung verwenden. 
    - Auf lokale Ressource wird automatisch konfiguriert, um die primäre auf lokale Verbindungszeichenfolge verwenden.

- Hybrid Verbindungs-Managers muss eine gültige lokalen Verbindungszeichenfolge für die Autorisierung verwenden. Die Azure Web Apps oder Mobile-Apps muss eine gültige Anwendung Verbindungszeichenfolge für die Autorisierung verwenden.
- Sie können Verbindungen Hybrid skalieren, nach der Installation auf einem anderen Server von einer anderen Instanz des Hybrid-Verbindungs-Managers. Konfigurieren der lokalen Zuhörer um derselben Adresse als der ersten lokalen Zuhörer verwenden. In diesem Fall ist der Datenverkehr zufällig verteilte (Round Robert) zwischen den aktiven lokalen Listenern. 


## <a name="a-namemanagehybridconnectionamanage-hybrid-connections"></a><a name="ManageHybridConnection"></a>Verwalten von Hybrid-Verbindungen
Zum Verwalten Ihrer Hybrid-Verbindungen können Sie folgende Aktionen ausführen:

- Verwenden Sie des Azure-Portals, und wechseln Sie zu Ihrem BizTalk Service. 
- Verwenden Sie [REST-APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopieren/Regenerate die Zeichenfolgen in Verbindung

1. Melden Sie sich zum [Azure klassischen Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)aus.
2. Klicken Sie im linken Navigationsbereich wählen Sie **BizTalk-Dienste aus** , und wählen Sie dann auf Ihre BizTalk Service. 
3. Wählen Sie die Registerkarte **Hybrid Verbindungen** aus:  
![Registerkarte Hybrid-Verbindungen][HybridConnectionTab]
4. Wählen Sie die Verbindung Hybrid aus. Wählen Sie in der Taskleiste **Verbindung verwalten**:  
![Verwalten von Optionen][HCManageConnection]

    **Verwalten von Verbindung** eine Liste der Anwendung und lokale Verbindungszeichenfolgen. Können Sie die Verbindungszeichenfolgen kopieren oder neu erstellt die Zugriffstaste in der Verbindungszeichenfolge verwendet. 

    **Wenn Sie Regenerieren auswählen**, die in der Verbindungszeichenfolge verwendet Zugriffstaste freigegeben wird geändert. Gehen Sie wie folgt vor:
    - Wählen Sie im Portal Azure klassischen **Synchronisieren Tasten** in der Azure-Anwendung.
    - Führen Sie die **Lokale Setup**erneut aus. Wenn Sie die Einrichtung auf lokale erneut ausführen, ist die lokale Ressource automatisch für den aktualisierten primären Verbindungszeichenfolge verwendet konfiguriert.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Verwenden von Gruppenrichtlinien zum Steuern, der von einer Verbindung Hybrid verwendeten lokalen Ressourcen

1. Herunterladen der [Hybrid-Verbindungs-Managers Administrative Vorlagen](http://www.microsoft.com/download/details.aspx?id=42963).
2. Die Dateien zu extrahieren.
3. Klicken Sie auf dem Computer, Gruppenrichtlinien ändert, gehen Sie folgendermaßen vor:  

    - Kopieren der. ADMX-Dateien in den Ordner *%WINROOT%\PolicyDefinitions* .
    - Kopieren der. ADML-Dateien in den Ordner *%WINROOT%\PolicyDefinitions\en-us* .

Nachdem Sie kopiert haben, können Gruppenrichtlinien-Editor Sie um die Richtlinie zu ändern.




## <a name="next"></a>Weiter

[Herstellen einer Verbindung eine lokale Ressource mit Azure Web Apps](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Verbinden Sie mit lokalen SQLServer aus Azure Web Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hybrid Verbindungen (Übersicht)](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Siehe auch

[REST-API für die Verwaltung von BizTalk-Dienste auf Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md)  
[Erstellen einer BizTalk Service mithilfe von Azure klassischen portal](biztalk-provision-services.md)  
[BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
