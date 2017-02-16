<properties
   pageTitle="Fehlerbehandlung Azure Automatisierung | Microsoft Azure"
   description="Dieser Artikel enthält grundlegende Fehler Behandlung Schritte zur Problembehandlung beheben häufig auftretender Azure Automatisierung Fehler an."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="Automatisierungsfehler, Fehlerbehandlung"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Fehlerbehandlung Tipps zum häufigen Azure Automatisierung

In diesem Artikel werden einige der Azure Automatisierung häufigen, die Sie möglicherweise erzielen und schlägt vor möglichen Fehlerbehandlung Schritte, erläutert.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Problembehandlung bei Fehlern bei der Authentifizierung, bei der Arbeit mit Azure Automatisierung runbooks  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Szenario: Fehler beim Azure-Konto anmelden

**Fehler:** Die Fehlermeldung "Unknown_user_type: Unbekannte Benutzertyp" bei der Arbeit mit der hinzufügen-AzureAccount oder Login-AzureRmAccount Cmdlets.

**Grund für den Fehler:** Dieser Fehler tritt auf, wenn der Name der Anlage Anmeldeinformationen nicht gültig ist oder wenn Sie den Benutzernamen und das Kennwort ein, mit dem Sie die Anlage Automatisierung Anmeldeinformationen für die Einrichtung, nicht gültig sind.

**Tipps zur Problembehandlung:** Um zu ermitteln, was falsch ist, gehen Sie folgendermaßen vor:  

1. Stellen Sie sicher, dass Ihnen keine Sonderzeichen, einschließlich der **@** in die Automatisierung Anmeldeinformationen Name der Anlage, die Sie verwenden, um die Verbindung mit Azure-Zeichen.  

2. Überprüfen Sie, dass Sie verwenden können, den Benutzernamen und das Kennwort ein, die in der Azure Automatisierung Anmeldeinformationen in Ihrem lokalen PowerShell ISE Editor gespeichert werden. Sie können dazu die folgenden Cmdlets in der PowerShell ISE ausgeführt:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Wenn Ihre Authentifizierung lokal fehlschlägt, bedeutet dies, dass Sie Ihre Anmeldeinformationen Azure Active Directory ordnungsgemäß eingerichtet haben. Finden Sie unter [Authenticating in Azure mit Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) Blogbeitrag zum Abrufen des Azure-Active Directory-Kontos ordnungsgemäß eingerichtet.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Szenario: Nicht gefunden Azure-Abonnement

**Fehler:** Sie erhalten die Fehlermeldung "das Abonnement mit dem Namen ``<subscription name>`` wurde nicht gefunden" bei der Arbeit mit der Select-AzureSubscription oder auswählen-AzureRmSubscription Cmdlets.

**Grund für den Fehler:** Dieser Fehler tritt auf, wenn der Abonnementname ungültig ist oder der Azure-Active Directory-Benutzer, der versucht, zu der Seite Abonnementdetails erhalten als Administrator des Abonnements konfiguriert ist.

**Tipps zur Problembehandlung:** Um festzustellen, ob Sie ordnungsgemäß in Azure authentifiziert wurden und haben Zugriff auf das Abonnement, das Sie markieren möchten, führen Sie die folgenden Schritte aus:  

1. Stellen Sie sicher, dass Sie der **Hinzufügen-AzureAccount** ausführen, bevor Sie das Cmdlet **AzureSubscription auswählen** .  

2. Wenn diese Fehlermeldung weiterhin angezeigt wird, ändern Sie den Code, indem Sie das Cmdlet " **Get-AzureSubscription** " das **Hinzufügen-AzureAccount** -Cmdlet folgen hinzufügen, und klicken Sie dann führen Sie den Code aus.  Jetzt überprüfen Sie, wenn die Ausgabe der Get-AzureSubscription Ihre Abonnementdetails enthält.  
    * Wenn Sie alle Abonnementdetails in der Ausgabe nicht angezeigt werden, bedeutet dies, dass es sich bei noch Initialisierung des Abonnements nicht zur Verfügung.  
    * Wenn Sie in der Ausgabe der Seite Abonnementdetails angezeigt werden, vergewissern Sie sich, dass Sie den richtigen Abonnementname oder die ID mit der **Select-AzureSubscription** -Cmdlet verwenden.   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Szenario: Azure-Authentifizierung fehlgeschlagen ist, da die kombinierte Authentifizierung aktiviert ist

**Fehler:** Sie erhalten die Fehlermeldung "hinzufügen-AzureAccount: AADSTS50079: strenge Authentifizierung Registrierung (Nachweis oben) ist erforderlich" bei der Authentifizierung in Azure mit Ihren Azure-Benutzernamen und Ihr Kennwort ein.

**Grund für den Fehler:** Wenn Sie eine kombinierte Authentifizierung auf Ihrem Azure-Konto verfügen, können Azure-Active Directory-Benutzer Sie in Azure authentifizieren.  Stattdessen müssen Sie ein Zertifikat oder einem Dienst Hauptbenutzer zur Azure Authentifizierung verwenden.

**Tipps zur Problembehandlung:** Wenn Sie ein Zertifikat mit den Servicemanagement Azure-Cmdlets verwenden zu können, finden Sie in [Erstellen und Hinzufügen eines Zertifikats zum Verwalten von Azure Services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Um einen Dienst Tilgungsanteile mit Azure Ressourcenmanager Cmdlets verwenden zu können, finden Sie in [Service Tilgungsanteile mit Azure-Portal erstellen](./resource-group-create-service-principal-portal.md) und [Authentifizierung ein Dienst Tilgungsanteile Azure Ressourcenmanager.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Problembehandlung bei häufigen Fehlern bei der Arbeit mit runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Szenario: Runbooks schlägt aufgrund deserialisierte Objekt

**Fehler:** Ihre Runbooks schlägt fehl, und der Fehler "kann nicht Parameter binden ``<ParameterName>``. Nicht konvertieren der ``<ParameterType>`` Wert des Typs Deserialized ``<ParameterType>`` zum Eingeben von ``<ParameterType>``".

**Grund für den Fehler:** Ist Ihre Runbooks eines Workflows PowerShell, speichert komplexe Objekte in einem deserialisierten Format akzeptieren, um Ihre Runbooks Zustand beibehalten werden, wenn der Workflow angehalten wird.  

**Tipps zur Problembehandlung:**  
Eine der folgenden drei Solutions wird dieses Problem zu beheben:

1. Wenn Sie komplexe Objekte von einem Cmdlet Rohrleitungsplan sind, umbrechen dieser Cmdlets in einer InlineScript auf.  
2. Übergeben Sie den Namen oder den gewünschten Wert aus der komplexen Objekts anstelle des gesamten Objekts übergeben.  

3. Verwenden eines PowerShell Runbooks anstelle eines Runbooks PowerShell-Workflow an.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Szenario: Runbooks Auftrag fehlgeschlagen, da das zugewiesene Kontingent überschritten

**Fehler:** Ihre Arbeit Runbooks, schlägt der Fehler "das Kontingent für die monatlichen total Auftrag Laufzeit für dieses Abonnement erreicht wurde".

**Grund für den Fehler:** Dieser Fehler tritt auf, wenn die Ausführung des Auftrags 500-minütiges kostenlosen Kontingents für Ihr Konto überschreitet. Dieses Kontingent gilt für alle Typen von Projektaufgaben der Ausführung ein Auftrags testen, Starten eines Auftrags aus dem Portal, Ausführung eines Auftrags durch Webhooks verwenden und Planen von Aufträgen auszuführende mit dem Azure Portal oder im Datencenter. Erfahren Sie, finden Sie weitere Informationen zu Preise für Automatisierung [Automatisierung Preise](https://azure.microsoft.com/pricing/details/automation/).

**Tipps zur Problembehandlung:** Wenn Sie mehr als 500 Minuten Verarbeitung pro Monat verwenden möchten, müssen so ändern Sie Ihr Abonnement über das kostenlose Ebene an die grundlegende Schicht. Sie können auf die grundlegende Ebene aktualisieren, indem Sie die folgenden Schritte aus:  

1. Melden Sie sich bei Ihrem Azure-Abonnement  
2. Wählen Sie das Automatisierung-Konto, das Sie aktualisieren möchten.  
3. Klicken Sie auf **Einstellungen**auf > **Preise Ebene und die Verwendung** > **Preise Ebene**  
4. Wählen Sie auf das **Auswählen der Preisgestaltung Ebene** Blade **grundlegende**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Szenario: Cmdlet nicht erkannt beim Ausführen einer Runbooks

**Fehler:** Ihre Arbeit Runbooks schlägt mit dem Fehler "``<cmdlet name>``: den Ausdruck ``<cmdlet name>`` wird als den Namen eines Cmdlet, Funktion, Skriptdatei oder aktivierbar Programm nicht erkannt."

**Grund für den Fehler:** Dieser Fehler wird verursacht, wenn die PowerShell-Engine das Cmdlet nicht finden kann, die, das Sie in Ihrem Runbooks verwenden.  Möglicherweise das Modul, enthält das Cmdlet fehlt in das Konto, ein Konflikt mit einem Namen Runbooks vorhanden ist oder das Cmdlet auch vorhanden ist, in einem anderen Modul und Automatisierung der Name kann nicht aufgelöst werden.

**Tipps zur Problembehandlung:** Eine der folgenden Lösungen wird das Problem beheben:  

- Überprüfen Sie, dass Sie das Cmdlet Name korrekt eingegeben haben.  

- Vergewissern Sie sich das Cmdlet in Ihr Konto Automatisierung vorhanden ist, dass keine Konflikte vorhanden sind. Um zu überprüfen, wenn das Cmdlet vorhanden ist, öffnen Sie eine Runbooks Bearbeitungsmodus und eine Suche für das Cmdlet finden in der Bibliothek oder ausführen soll **Get-Befehl ``<CommandName>`` **.  Sobald Sie haben überprüft, die das Cmdlet mit dem Konto verfügbar ist, und es keine Namenskonflikte mit anderen Cmdlets oder Runbooks sind, Lizenz zu den Zeichenbereich, und stellen Sie sicher, dass Sie in Ihrer Runbooks festlegen gültigen Parameter verwenden.  

- Wenn Sie einen Namenskonflikt verfügen und das Cmdlet in zwei verschiedenen Modulen steht, können Sie dieses Verhalten zu beheben, indem Sie den vollqualifizierten Namen für das Cmdlet mit. Beispielsweise können Sie **ModuleName\CmdletName**verwenden.  

- Wenn Sie die Runbooks lokal in der Gruppe Worker Hybrid ausführen möchten, stellen Sie sicher, dass das Modul-Cmdlet auf dem Computer installiert ist, der die Hybrid Worker hostet.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Szenario: Eine zeitintensive Runbooks konsistente schlägt fehl, mit der Ausnahme: "der Auftrag kann nicht ausgeführt werden, da es wiederholt vom gleichen Prüfpunkt entfernt wurde fortgesetzt".

**Grund für den Fehler:** Dies ist durch Entwurf Verhalten aufgrund der "Gleich großen Anteil" Überwachung der-Prozesse auf Azure-Automatisierung, die automatisch eine Runbooks ausgesetzt wird, wenn sie mehr als 3 Stunden ausgeführt wird. Die Fehlermeldung zurückgegeben, bietet jedoch keine "wie geht es weiter" Optionen. Eine Runbooks kann eine Reihe von Gründen unterbrochen werden. Hält passiert hauptsächlich aufgrund von Fehlern. Beispielsweise nicht abgefangen verursacht eine Ausnahme in einer Runbooks, ein Fehler bei der oder einem Absturz auf die Ausführung des Runbooks Runbooks Worker, alle des Runbooks unterbrochen werden und diese dann aus den letzten Prüfpunkt Wenn fortgesetzt wird.

**Tipps zur Problembehandlung:** Die dokumentierte Lösung, um dieses Problem zu vermeiden sollten Kontrollpunkten in einem Workflow verwenden.  Weitere Informationen finden Sie weitere [Learning PowerShell](automation-powershell-workflow.md#Checkpoints)Workflows.  Eine genauere Erläuterung von "Gleich großen Anteil" und Wissensstand kann in diesem Artikel Blog [Mithilfe von Kontrollpunkten in Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/)gefunden werden.


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Problembehandlung bei häufigen beim Module importieren

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Szenario: Modul kann keine Verbindung zu importieren oder Cmdlets kann nicht ausgeführt werden, nach dem Import

**Fehler:** Ein Modul kann keine Verbindung zu importieren oder erfolgreich importiert, aber keine Cmdlets extrahiert werden.

**Grund für den Fehler:** Einige häufige Gründe, die ein Modul zu Azure Automatisierung nicht erfolgreich importiert möglicherweise sind:  

- Die Struktur entspricht nicht die Struktur, die Automatisierung sein erforderlich sind.  

- Das Modul hängt von einem anderen Modul, das nicht mit Ihrem Konto Automatisierung bereitgestellt wurde.  

- Das Modul fehlt Abhängigkeit davon in den Ordner.  

- Das Cmdlet " **New-AzureRmAutomationModule** " wird verwendet, um das Modul hochladen, und Sie haben den vollständige Speicherpfad gewährt oder des Moduls mithilfe einer öffentlich zugänglichen URL nicht geladen haben.  

**Tipps zur Problembehandlung:**  
Eine der folgenden Lösungen wird das Problem beheben:  

- Stellen Sie sicher, dass das Modul folgende Format folgt:  
ModuleName.Zip **->** ModuleName oder Versionsnummer **->** (ModuleName.psm1, ModuleName.psd1)

- Öffnen Sie die Datei .psd1, und sehen Sie, ob das Modul Abhängigkeiten hat.  Ist dies der Fall, laden Sie diese Module mit dem Konto Automatisierung hoch.  

- Stellen Sie sicher, dass alle DLL verwiesen wird im Modul Ordner vorhanden sind.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Problembehandlung bei häufigen Fehlern bei der Arbeit mit gewünscht Zustand Konfiguration (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Szenario: Knoten ist in einem Fehlerstatus mit einem Fehler "Nicht gefunden"

**Fehler:** Der Knoten wurde einen Bericht mit Status **fehlgeschlagen** und mit dem Fehler "die Aktion vom Server https:// Abrufen``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction ist fehlgeschlagen, weil eine gültige Konfiguration ``<guid>`` kann nicht gefunden werden."

**Grund für den Fehler:** Dieser Fehler tritt in der Regel bei der Knoten, um einen Namen für die (z. B. ABC zugeordnet ist) statt einen Namen für die Knoten (z. B. ABC. WebServer).  

**Tipps zur Problembehandlung:**  

- Stellen Sie sicher, dass Sie den Knoten mit "Knoten Konfigurationsname" und nicht die "Konfigurationsname" zuweisen.  

- Sie können eine Knotenkonfiguration zu einem Knoten mit Azure-Portal oder mit PowerShell-Cmdlet zuweisen.
    - Akzeptieren, um eine Knotenkonfiguration zu einem Knoten mit Azure-Portal zuweisen möchten, öffnen Sie das Blade **DSC Knoten** , und klicken Sie dann wählen Sie einen Knoten aus, und klicken Sie auf die Schaltfläche **Knotenkonfiguration zuweisen** .  
    - Um eine Knotenkonfiguration zu einem Knoten mit PowerShell-Cmdlet zuweisen möchten, verwenden Sie Cmdlet " **Set-AzureRmAutomationDscNode** "


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Szenario: Keine Knoten Konfigurationen (MOF-Dateien) wurden erstellt, wenn eine Konfiguration kompiliert wurde

**Fehler:** Die Position der DSC Kompilierung ausgesetzt, mit dem Fehler: "Kompilierung wurde erfolgreich abgeschlossen, aber es wurden keine Knoten Konfiguration .mofs generiert".

**Grund für den Fehler:** Wenn der Ausdruck nach, die das Schlüsselwort **Knoten** in der Konfiguration DSC wertet $null und dann keine Knoten Konfigurationen erzeugt werden.    

**Tipps zur Problembehandlung:**  
Eine der folgenden Lösungen wird das Problem beheben:  

- Stellen Sie sicher, dass der Ausdruck neben das Schlüsselwort **Knoten** in der Konfigurationsdefinition nicht auf $null auswertet.  
- Wenn Sie beim Kompilieren der Konfigurations ConfigurationData übergeben, stellen Sie sicher, dass Sie die erwarteten Werte, die die Konfiguration erfordert von [ConfigurationData](automation-dsc-compile.md#configurationdata)übergeben werden.


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Szenario: DSC Knoten Bericht wird Zustand "in Bearbeitung" hängen.

**Fehler:** DSC Agent gibt "Keine Instanz mit vorhandenen Eigenschaftswerte gefunden."

**Grund für den Fehler:** Sie haben Ihrer WMF-Version aktualisiert und haben WMI beschädigt.  

**Tipps zur Problembehandlung:** Folgen Sie den Anweisungen im Blogbeitrag [DSC bekannte Probleme und Einschränkungen](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) , das Problem zu beheben.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Szenario: Keine Anmeldeinformationen in einer DSC Konfiguration verwenden

**Fehler:** Die Position der DSC Kompilierung mit dem Fehler unterbrochen wurde: "System.InvalidOperationException Fehler bei der Bearbeitung der Eigenschaft 'Anmeldeinformationen' vom Typ ``<some resource name>``: konvertieren und Speichern von einem verschlüsselten Kennwort als nur-Text zulässig ist, wenn PSDscAllowPlainTextPassword festgelegt haben true".

**Grund für den Fehler:** Sie haben einen Eintrag in einer Konfiguration verwendet aber nicht bereitstellen gemischte **ConfigurationData** zum **PSDscAllowPlainTextPassword** true für jede Knotenkonfiguration festlegen.  

**Tipps zur Problembehandlung:**  
- Vergewissern Sie sich in der richtigen **ConfigurationData** für **PSDscAllowPlainTextPassword** festzulegen, für die einzelnen Knoten Varianten in der Konfiguration angegeben ist WAHR übergeben. Weitere Informationen finden Sie in der [Vermögenswerte in Azure Automatisierung DSC](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die vorstehenden Schritte zur Problembehandlung befolgt haben und an einer beliebigen Stelle in diesem Artikel zusätzliche Hilfe benötigen, können Sie:

- Abrufen von Hilfe bei Azure-Experten. Das Problem zum Senden der [MSDN Azure oder Stapelüberlauf Foren.](https://azure.microsoft.com/support/forums/)

- Einen Supportvorfall Azure--Datei. Wechseln Sie zu der [Website Azure unterstützen](https://azure.microsoft.com/support/options/) , und klicken Sie unter **technischen Support und Abrechnungssupport**auf **Unterstützung** .

- Posten Sie ein Skript anfordern [Script](https://azure.microsoft.com/documentation/scripts/) Center, wenn Sie eine Azure Automatisierung Runbooks Lösung oder ein Integrationsmodul suchen.

- Posten Sie Feedback oder Feature Besprechungsanfragen für Azure Automatisierung auf [Benutzer Voicemail](https://feedback.azure.com/forums/34192--general-feedback).
