<properties
   pageTitle="Fortlaufende Integration für Dienst Fabric | Microsoft Azure"
   description="Eine Übersicht über zum Einrichten der fortlaufenden Integration für eine Fabric Service-Anwendung mithilfe von Visual Studio Team Services (VSTS)."
   services="service-fabric"
   documentationCenter="na"
   authors="mthalman-msft"
   manager="timlt"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/01/2016"
   ms.author="mthalman" />

# <a name="set-up-continuous-integration-for-a-service-fabric-application-by-using-visual-studio-team-services"></a>Einrichten von fortlaufende Integration für eine Fabric Service-Anwendung mithilfe von Visual Studio Team Services

In diesem Artikel werden die Schritte zum Einrichten von fortlaufende Integration für eine Anwendung Azure Service Fabric mithilfe von Visual Studio Team Services (VSTS), um sicherzustellen, dass eine Anwendung erstellt, verpackt und auf automatisierte Weise bereitgestellt wird.

Dieses Dokument die aktuelle Prozedur widerspiegelt und soll im Laufe der Zeit ändern.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um anzufangen, gehen Sie folgendermaßen vor:

1. Stellen Sie sicher, dass Sie Zugriff auf eine Teamwebsite Services-Kontos oder [Erstellen Sie eine](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) selbst haben.

2. Stellen Sie sicher, dass Sie Zugriff auf ein Team Services Team Project oder [Erstellen Sie eine](https://www.visualstudio.com/docs/setup-admin/create-team-project) selbst haben.

3. Stellen Sie sicher, dass Sie einen Dienst Fabric Cluster verfügen, den Sie Ihrer Anwendung bereitstellen oder eine mithilfe der [Azure-Portal](service-fabric-cluster-creation-via-portal.md), eine [Vorlage Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md)oder [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md)erstellen können.

4. Stellen Sie sicher, dass Sie bereits ein Projekt Fabric Service-Anwendung (.sfproj) erstellt haben. Sie müssen an einem Projekt arbeiten, die erstellt oder mit Service Fabric SDK 2.1 oder höher (die Datei .sfproj sollte einen Eigenschaftswert ProjectVersion 1.1 oder höher enthalten) aktualisiert wurde.

>[AZURE.NOTE] Benutzerdefinierte Build-Agents sind nicht mehr erforderlich. Team Services gehostet wird für die Bereitstellung der Anwendung direkt aus diesen Agents jetzt im Zusammenhang mit dem Dienst Fabric Cluster Management-Software vorinstalliert Agents.

## <a name="configure-and-share-your-source-files"></a>Konfigurieren und Freigeben von Quelldateien

Erstes erhalten möchten Sie tun ist ein Veröffentlichungsprofil für die Verwendung durch den Bereitstellungsprozess vorzubereiten, die innerhalb von Team Services ausgeführt wird.  Das Veröffentlichungsprofil sollten um Cluster zu adressieren, den Sie zuvor vorbereitet haben konfiguriert sein:

1.  Wählen Sie ein Veröffentlichungsprofil in Projekt Anwendung, das Sie für den Workflow fortlaufende Integration verwenden, und folgen den [Anweisungen veröffentlichen](service-fabric-publish-app-remote-cluster.md) , wie eine Anwendung zu einem remote Cluster veröffentlichen möchten. Tatsächlich müssen Sie nicht durch die Anwendung veröffentlichen. Nachdem Sie die Punkte ordnungsgemäß konfiguriert haben, können Sie einfach den Hyperlink **Speichern** im Dialogfeld "Veröffentlichen" klicken.
2.  Wenn Sie möchten eine Anwendung für jede Bereitstellung aktualisiert werden, die innerhalb der Team Services auftritt, werden Sie das Veröffentlichungsprofil, um Aktualisierung aktivieren konfigurieren möchten. Im selben veröffentlichen Dialogfeld in Schritt 1 verwendet werden Stellen Sie sicher, dass das Kontrollkästchen **Upgrade der Anwendung** aktiviert ist.  Weitere Informationen zum [Konfigurieren von zusätzlichen Upgradeeinstellungen](service-fabric-visualstudio-configure-upgrade.md). Klicken Sie auf den Link **Speichern** , um die Einstellungen für das Veröffentlichungsprofil zu speichern.
3.  Stellen Sie sicher, dass Sie Ihre Änderungen, um das Veröffentlichungsprofil gespeichert haben und das Dialogfeld ' veröffentlichen ' Abbrechen.
4.  Nun ist es an der Zeit zum [Freigeben Ihrer Anwendung Quelle Projektdateien](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) mit Team Services. Nachdem Sie Ihre Quelldateien im Team Services zugänglich sind, können Sie jetzt mit dem nächsten Schritt generieren Builds verschieben, klicken Sie auf. 

## <a name="create-a-build-definition"></a>Erstellen einer Definition erstellen

Team Services Builddefinition beschrieben, einen Workflow, der eine Reihe von Schritten erstellen besteht, die sequenziell ausgeführt werden. Das Ziel der Definition erstellen, die Sie erstellen können ist Paket einer Fabric Service-Anwendung sowie einige andere ergänzenden Dateien aus, die verwendet werden können, die Anwendung zu einem Cluster später bereitstellen einschließlich berechnet werden. Weitere Informationen zu Team Services [Definitionen erstellen](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-the-build-template"></a>Erstellen Sie eine Definition aus der Vorlage erstellen

1.  Öffnen Sie Ihr Team Project in Visual Studio Team Services.
2.  Wählen Sie die Registerkarte **Erstellen** .
3.  Wählen Sie die grüne **+** anmelden, um eine neue Builddefinition zu erstellen.
4.  Wählen Sie im Dialogfeld, das geöffnet wird, **Azure Fabric Anwendungsdienst** in die Vorlagenkategorie **Erstellen** aus.
5.  Wählen Sie **Weiter**aus.
6.  Wählen Sie Repository und Verzweigung Ihrer Anwendung Dienst Fabric zugeordnet.
7.  Wählen Sie die Agentwarteschlange, die Sie verwenden möchten. Gehostete Agents werden unterstützt.
8.  Wählen Sie auf **Erstellen**.
9. Speichern Sie die Definition erstellen, und geben Sie einen Namen ein.
10. Im folgenden finden eine Beschreibung der von der Vorlage generierte Build Schritte aus:

| Erstellen der Schritt | Beschreibung |
| --- | --- |
| NuGet wiederherstellen | Stellt die NuGet-Pakete für die Lösung. |
| Erstellen der Lösung \*.sln | Erstellt die gesamte Lösung. |
| Erstellen der Lösung \*.sfproj | Generiert Paket der Fabric Service-Anwendung, das zum Bereitstellen der Anwendung verwendet wird. Beachten Sie, dass der Speicherort des Pakets Datenverzeichnis Element des Build werden angegeben ist. |
| Aktualisieren der Dienst Fabric App-Versionen | Aktualisiert die Versionswerte enthalten, in das Anwendungspaket Manifestdateien zum Upgrade Support zulässig ist. Finden Sie in der [Dokumentation Aufgabenseite](https://go.microsoft.com/fwlink/?LinkId=820529) für Weitere Informationen. |
| Kopieren Sie die Dateien | Kopiert die Veröffentlichungsdateien Profil und die Anwendung Parameter zu dem Erstellen der Elemente und für die Bereitstellung genutzt werden. |
| Element veröffentlichen | Veröffentlicht das Erstellen der Elemente. Dadurch wird die Release Definition auf das Erstellen der Elemente zu nutzen. |

### <a name="verify-the-default-set-of-tasks"></a>Überprüfen Sie die Standardgruppe von Aufgaben

1.  Vergewissern Sie sich im Eingabefeld **Lösung** für das **NuGet wiederherstellen** und Erstellen von Schritten **Lösung erstellen** .  Standardmäßig wird diese Schritte bei allen enthaltenen Lösungsdateien, in dem entsprechenden Repository führt erstellen.  Wenn nur die Definition erstellen auf eine dieser Lösung Dateien angewendet werden soll, müssen Sie den Pfad explizit in dieser Datei zu aktualisieren.
2.  Überprüfen Sie das **Lösung** Eingabefeld für das **Paket Anwendung** Erstellungschritt.  Dieser Schritt erstellen geht standardmäßig davon aus, dass nur ein Fabric Anwendungsdienst Projekt (.sfproj) im Repository vorhanden ist.  Wenn Sie mehrere solche Dateien in Ihrem Repository haben und nur eine von ihnen für diese Builddefinition Ziel festlegen möchten, müssen Sie den Pfad explizit in dieser Datei zu aktualisieren.  Wenn Sie mehrere Anwendungsprojekte in Ihrem Repository packen möchten, müssen Sie zusätzliche Schritte mit **Visual Studio erstellen** in der Definition erstellen erstellen, jeweils ein Anwendungsprojekt Zielen.  Sie möchten, und klicken Sie dann auch das Feld **MSBuild-Argumente** für jede dieser aktualisieren müssen Schritte erstellen, damit der Speicherort des Pakets für jede von ihnen eindeutig ist.
3.  Stellen Sie sicher das Versionierungsverhalten **Update-Dienst Fabric App-Versionen** erstellen Schritt definiert.  Standardmäßig fügt dieses Schritts erstellen die Build-Nummer an alle Werte für Version in das Anwendungspaket Manifestdateien an. Finden Sie in der [Dokumentation Aufgabenseite](https://go.microsoft.com/fwlink/?LinkId=820529) für Weitere Informationen. Dies ist sinnvoll für die Unterstützung von Upgrade Ihrer Anwendung, da jede Upgrade-Bereitstellung andere Versionswerte aus der vorherigen Bereitstellung erforderlich ist. Wenn Sie nicht die Absicht sind, Anwendungsupgrade in den Workflow verwenden, könnten Sie diesen Schritt erstellen deaktivieren. Tatsächlich muss deaktiviert werden, wenn Ihre Absicht besteht darin, eine erstellen, die verwendet werden können, um eine vorhandene Fabric Service-Anwendung zu überschreiben, da Bereitstellung fehl, wenn die Version der vom Build erstellten Anwendung nicht die Version der Anwendung im Cluster übereinstimmt.
4.  Wenn Ihre Lösung ein Projekts .NET Core enthält, müssen Sie sicherstellen, dass der Definition Ihrer erstellen ein Schritts erstellen enthält, das die Abhängigkeiten von Dateien project.json definierten stellt.  Gehen Sie dazu folgendermaßen vor:
   1. Wählen Sie **hinzufügen erstellen... Schritt**aus.
   2. Suchen Sie nach **Befehlszeile** Vorgangs innerhalb der Registerkarte Programm, und klicken Sie auf die Schaltfläche hinzufügen.
   3. Verwenden Sie für die Eingabewerte Vorgangsfelder die folgenden Werte ein:
      1. Tool: Dotnet
      2. Argumente: Wiederherstellen
   4. Ziehen Sie den Vorgang an, sodass sie unmittelbar nach dem Schritt **NuGet wiederherstellen** ist.
5.  Auf die Generator-Definition vorgenommenen Änderungen zu speichern.

### <a name="try-it"></a>Probieren Sie es

Wählen Sie einen Build manuell starten **Warteschlange erstellen** . Builds werden auch bei Pushbenachrichtigungen oder Einchecken ausgelöst werden. Nachdem Sie überprüft haben, dass der Build erfolgreich ausgeführt wird, können Sie jetzt navigieren Sie zum definieren, die Ihrer Anwendung zu einem Cluster bereitstellen Release Definition.

## <a name="create-a-release-definition"></a>Erstellen einer Version definition

Team Services Release Definition beschreibt einen Workflow, der eine Reihe von Vorgängen besteht, die sequenziell ausgeführt werden. Das Ziel der Definition der Version, die Sie erstellen werden, werden ist ein Anwendungspaket übernehmen und es zu einem Cluster bereitstellen. Wenn gemeinsam verwendet werden, können die Definition erstellen und Release Definition den gesamten Workflow aus Quelldateien an, die mit einer laufenden Anwendung in Ihren Cluster angefangen ausführen. Weitere Informationen zu Team Services [Version Definitionen](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-the-release-template"></a>Erstellen Sie eine Definition aus der Vorlage Version

1.  Öffnen des Projekts in Visual Studio Team Services an.
2.  Wählen Sie die Registerkarte **Version** .
3.  Wählen Sie die grüne **+** erstellen eine neue Version Definition, und wählen Sie im Menü **Erstellen Release Definition** melden.
4.  Wählen Sie im Dialogfeld, das geöffnet wird, **Bereitstellung von Azure Service Fabric** innerhalb der **Bereitstellung** Vorlagenkategorie aus.
5.  Wählen Sie **Weiter**aus.
6.  Wählen Sie die Definition von erstellen, die Sie als Quelle dieser Version Definition verwenden möchten.  Die veröffentlichte Version Definition wird die Elemente verwiesen werden, die von der ausgewählten Builddefinition erstellt wurden.
7.  Aktivieren Sie das Kontrollkästchen **fortlaufende Bereitstellung** Wunsch Team Services automatisch eine neue Version erstellen und Bereitstellen der Fabric Service-Anwendungs, wenn ein Build abgeschlossen wurde.
8.  Wählen Sie die Agentwarteschlange, die Sie verwenden möchten. Gehostete Agents werden unterstützt.
9.  Wählen Sie auf **Erstellen**.
10. Bearbeiten Sie den Definitionsnamen, indem Sie auf das Bleistiftsymbol am oberen Rand der Seite.
11. Wählen Sie aus, dem die Anwendung sollen, aus dem **Cluster Verbindung** Eingabefeld des Vorgangs bereitgestellt werden, Cluster. Die Verbindung Cluster stellt die erforderlichen Informationen, die die Bereitstellungsaufgabe Verbindung mit dem Cluster ermöglicht. Wenn Sie noch nicht Cluster-Verbindung für Ihren Cluster haben, wählen Sie den Link **Verwalten** neben dem Feld hinzufügen. Klicken Sie auf der Seite, die angezeigt wird, führen Sie die folgenden Schritte aus:
    1. Wählen Sie **Neue Service-Endpunkts an** , und wählen Sie **Azure Service Fabric** aus dem Menü aus.
    2. Wählen Sie die Art der Authentifizierung vom Ziel von diesem Endpunkt Cluster verwendet wird.
    2. Definieren Sie einen Namen für die Verbindung im Feld **Verbindungsname** ein.  In der Regel, verwenden Sie den Namen der Cluster.
    3. Definieren der Client Verbindung Endpunkt-URL in das Feld **Cluster Endpunkt** an.  Beispiel: Https://contoso.westus.cloudapp.azure.com:19000.
    4. Definieren Sie für Azure-Active Directory-Anmeldeinformationen die Anmeldeinformationen, die Sie für die Verbindung zum Cluster in den Feldern **Benutzername** und **Kennwort** verwenden möchten.
    5. Definieren Sie für das Zertifikat formularbasierte Authentifizierung Base64-Codierung der Client Zertifikatsdatei im Feld **Client-Zertifikat** ein.  Finden Sie unter der Hilfe Popup auf die Informationen zum Abrufen des Werts im Feld ein.  Ist das Zertifikat durch ein Kennwort geschützte, definieren Sie das Kennwort im Feld **Kennwort** ein.
    6. Bestätigen Sie die Änderungen, indem Sie auf **OK**. Klicken Sie nach navigieren wieder zu der Definition Ihrer Version auf das Symbol "Aktualisieren", auf das Feld **Cluster-Verbindung,** um den Endpunkt anzuzeigen, die, den Sie soeben hinzugefügt haben.
12. Speichern Sie die veröffentlichte Version Definition.

Eine Aufgabe des Typs **Dienst Fabric Anwendung Bereitstellung**besteht aus die Definition, die erstellt wird. Finden Sie in der [Dokumentation Aufgabenseite](https://go.microsoft.com/fwlink/?LinkId=820528) für Weitere Informationen zu dieser Aufgabe.

### <a name="verify-the-template-defaults"></a>Überprüfen Sie die Vorlage Standardeinstellungen

1.  Überprüfen Sie das **Profil veröffentlichen** Eingabefeld für den Vorgang **Fabric-Anwendungsdienst bereitstellen** . Standardmäßig verweist dieses Feld ein Veröffentlichungsprofil mit dem Namen Cloud.xml in das Erstellen der Elemente enthalten sind. Wenn Sie Bezug auf ein Profil anderen veröffentlichen möchten oder den Build mehrere Anwendungspakete in deren Elemente enthält, müssen Sie den Pfad entsprechend zu aktualisieren.
2.  Vergewissern Sie sich das **Paket Anwendung** Eingabefeld für den Vorgang **Fabric-Anwendungsdienst bereitstellen** . Standardmäßig verweist dies den Standard-Paketpfad der Anwendung, die in der Vorlage erstellen Definition.  Wenn Sie den Pfad der Anwendung Standard-Paket in der Definition erstellen geändert haben, müssen Sie den Pfad ordnungsgemäß hier ebenfalls zu aktualisieren.

### <a name="try-it"></a>Probieren Sie es

Wählen Sie im Schaltfläche **lassen Sie wieder los** manuell erstellt von einer Version **Erstellen lassen Sie wieder los** . Wählen Sie den Build, den Sie als Grundlage für die Veröffentlichung, und klicken Sie dann auf **Erstellen**möchten, klicken Sie im Dialogfeld, das geöffnet wird. Wenn Sie kontinuierliche Bereitstellung aktiviert haben, werden Versionen Abschluss die zugeordneten Builddefinition einen Build auch automatisch erstellt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum fortlaufende Integration mit Dienst Fabric Applikationen lesen Sie die folgenden Artikeln:

 - [Team Services-Dokumentation Start](https://www.visualstudio.com/docs/overview)
 - [Erstellen von Management Team-Dienste](https://www.visualstudio.com/docs/build/overview)
 - [Lassen Sie wieder los Management Team-Dienste](https://www.visualstudio.com/docs/release/overview)
