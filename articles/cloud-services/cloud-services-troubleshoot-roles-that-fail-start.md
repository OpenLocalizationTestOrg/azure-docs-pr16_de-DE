<properties
   pageTitle="Behandeln von Problemen mit Rollen aus, die nicht gestartet werden | Microsoft Azure"
   description="Hier sind einige häufige Gründe, warum eine Rolle der Cloud-Dienst nicht gestartet werden kann. Außerdem werden Lösungen für diese Probleme bereitgestellt."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Behandeln von Problemen mit der Cloud-Dienst Rollen, die nicht gestartet werden

Hier sind einige häufig auftretende Probleme und Lösungen im Zusammenhang mit Azure Cloud Services Rollen aus, die nicht gestartet werden.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Fehlende DLL-Dateien oder Abhängigkeiten

Nicht reagiert Rollen und Rollen aus, die zwischen **Initialisierung**, **gebucht**und Staaten **Beenden** und ausschalten werden können, indem Sie fehlende DLL-Dateien oder Assemblys verursacht werden.

Fehlende DLL-Dateien oder Assemblys Symptome sind möglich:

- Ihre Rolleninstanz ist durch **Initialisierung**, **gebucht**und Staaten **Beenden** und ausschalten.
- Ihre Rolleninstanz wurde zu **bereit sind** , aber wenn Sie an Ihrer Webanwendung navigieren, wird die Seite nicht angezeigt.

Es gibt mehrere empfohlene Methoden zum Untersuchen diese Probleme.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Diagnostizieren Sie fehlende DLL-Probleme in einer Webrolle

Wenn Sie zu einer Website navigieren, die bereitgestellt wird in einem Web Rolle und im Browser zeigt einen Serverfehler ähnlich wie der folgende, es möglicherweise darauf hinzuweisen, dass eine DLL fehlt.

![Serverfehler in der Anwendung '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostizieren Sie Probleme durch Deaktivieren von benutzerdefinierten Fehlern

Genauere Fehlerinformationen kann diese Datei der Webrolle für den benutzerdefinierten Fehlermodus aus festzulegen, konfigurieren und erneutes Dienst angezeigt werden.

Genauere Fehler anzeigen, ohne die Verwendung von Remotedesktop:

1. Öffnen Sie die Lösung in Microsoft Visual Studio.

2. **Lösung-Explorer**suchen Sie die Datei web.config, und öffnen Sie es.

3. In der Datei web.config Suchen nach dem Abschnitt system.web, und fügen Sie die folgende Zeile hinzu:

    ```xml
    <customErrors mode="Off" />
    ```

4. Speichern Sie die Datei ein.

5. Packen und erneut den Dienst bereitstellen.

Nachdem Sie der Dienst neu bereitgestellt werden, wird eine Fehlermeldung mit dem Namen der fehlende Assembly oder DLL angezeigt.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Diagnostizieren Sie Probleme, indem Sie Remote-Anzeige des Fehlers

Remotedesktop können Sie die Rolle zugreifen und genauere Fehlerinformationen Remote anzeigen. Gehen Sie folgendermaßen vor, um der Fehler anzuzeigen, mit dem Remotedesktop:

1. Stellen Sie sicher, dass Azure SDK 1.3 oder höher installiert ist.

2. Während der Bereitstellung der Lösung mit Visual Studio wählen Sie "Konfigurieren Remote Desktop Verbindungen..." aus. Weitere Informationen zum Konfigurieren der Verbindung Remotedesktop finden Sie unter [Verwenden von Remotedesktop mit Azure Rollen](../vs-azure-tools-remote-desktop-roles.md).

3. In der klassischen Microsoft Azure-Portal, nachdem die Instanz Status **bereit**angezeigt wird, klicken Sie auf eine der Rolleninstanzen.

4. Klicken Sie im Bereich **RAS** des Menübands **Verbinden** .

5. Melden Sie sich am virtuellen Computer mit den Anmeldeinformationen, die während der Remote Desktop-Konfiguration angegeben wurden.

6. Öffnen Sie ein Eingabeaufforderungsfenster.

7. Typ `IPconfig`.

8. Notieren Sie den Wert für die IPv4-Adresse.

9. Öffnen Sie InternetExplorer.

10. Geben Sie die Adresse und den Namen der Website. Beispielsweise `http://<IPV4 Address>/default.aspx`.

Navigieren zu der Website gibt nun genauere Fehlermeldungen zurück:

* Serverfehler in der Anwendung '/'.

* Beschreibung: Ausnahmefehler während der Ausführung der aktuellen Anforderung Web. Überprüfen Sie den Stapel Spur für Weitere Informationen zu dem Fehler und Dateiursprung in den Code ein.

* Details der Ausnahme: System.IO.FIleNotFoundException: konnte nicht geladen, Datei oder Assembly ' Microsoft.WindowsAzure.StorageClient, Version = 1.1.0.0, Kultur = Neutral, PublicKeyToken = 31bf856ad364e35' oder eine Abhängigkeit davon. Die angegebene Datei wurde nicht gefunden.

Beispiel:

![Explizite Serverfehler in der Anwendung '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnostizieren von Problemen mithilfe der Serveremulator

Sie können die Microsoft Azure-Serveremulator zum Identifizieren und Beheben von Problemen der fehlende Abhängigkeiten und web.config Fehler verwenden.

Für optimale Ergebnisse bei Verwendung dieser Methode der Diagnose sollten Sie einem Computer oder virtuellen Computern, die über eine Neuinstallation von Windows verwenden. Um die Azure-Umgebung zu reproduzieren, verwenden Sie Windows Server 2008 R2 X64 aus.

1. Installieren der eigenständigen Version von [Azure SDK](https://azure.microsoft.com/downloads/)an.

2. Erstellen Sie auf dem Entwicklungscomputer Projekt für den Cloud-Dienst aus.

3. Navigieren Sie im Windows-Explorer zu dem Ordner Bin\debug des Projekts Cloud-Dienst.

4. Kopieren Sie die Datei .csx, Ordner und .cscfg auf dem Computer, den Sie verwenden, um die Probleme zu debuggen.

5. Öffnen Sie auf dem Computer säubern einer Azure SDK-Eingabeaufforderungsfenster und einem Typ `csrun.exe /devstore:start`.

6. Geben Sie an der Befehlszeile `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Wenn die Rolle gestartet wird, sehen Sie die Fehlerinformationen in Internet Explorer. Sie können auch standard Windows-Tools für die Problembehandlung verwenden, um das Problem zu diagnostizieren.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostizieren Sie Probleme mit IntelliTrace

Für Arbeits- und Webrollen, die .NET Framework 4 verwenden, können Sie [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx)verwenden, der in [Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs)verfügbar ist.

Wie folgt vor, um den Dienst mit aktiviertem IntelliTrace bereitgestellt:

1. Bestätigen Sie, dass Azure SDK 1.3 oder höher installiert ist.

2. Die Lösung mit Visual Studio bereit. Aktivieren Sie das Kontrollkästchen **IntelliTrace für .NET 4 Rollen aktivieren** während der Bereitstellung.

3. Sobald die Instanz gestartet wird, öffnen Sie den **Server-Explorer**.

4. Erweitern Sie die **Azure\\Cloud Services** Knoten und suchen Sie nach der Bereitstellung.

5. Erweitern Sie die Bereitstellung, bis die Rolleninstanzen angezeigt wird. Mit der rechten Maustaste auf eine der Instanzen.

6. Wählen Sie **Ansicht IntelliTrace Protokolle**aus. **IntelliTrace-Zusammenfassung** wird geöffnet.

7. Suchen Sie im Abschnitt Ausnahmen Zusammenfassung aus. Wenn es Ausnahmen gibt, ist im Abschnitt **Ausnahmedaten**gekennzeichnet.

8. Erweitern Sie die **Daten mit Ausnahme** und **System.IO.FileNotFoundException** Fehler etwa wie folgt aussehen:

![Ausnahmedaten, fehlende Datei oder assembly](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adressieren Sie fehlende DLL-Dateien und Assemblys

Um fehlende DLL und Zusammenstellungsfehler zu beheben, gehen Sie folgendermaßen vor:

1. Öffnen Sie die Lösung in Visual Studio.

2. Öffnen Sie im **Lösung Explorer**den Ordner **Verweise** aus.

3. Klicken Sie auf die Assembly, die den Fehler identifiziert.

4. Klicken Sie im Bereich **Eigenschaften** suchen Sie **Eigenschaft lokale Kopie** , und legen Sie den Wert **true**.

5. Erneut bereitstellen Sie Cloud-Dienst.

Nachdem Sie überprüft haben, dass alle Fehler behoben haben, können Sie den Dienst bereitstellen, ohne das Aktivieren des Kontrollkästchens **IntelliTrace für .NET 4 Rollen aktivieren** .

## <a name="next-steps"></a>Nächste Schritte

Weitere [Artikel zur Fehlerbehebung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Cloud Services anzeigen

Behebung von Cloud-Dienst Rolle mithilfe von Azure PaaS Computer Diagnosedaten finden Sie unter [Kevin Williamsons Blog Reihe](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
