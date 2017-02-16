<properties
    pageTitle="Melden Sie sich bei Azure, über die Befehlszeile | Microsoft Azure"
    description="Herstellen einer Verbindung mit Ihrem Abonnement Azure aus der Azure Line Interface (CLI Azure) für Mac, Linux und Windows"
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"
"/>

# <a name="log-in-to-azure-from-the-azure-cli"></a>Melden Sie sich bei Azure, über die Befehlszeile Azure

Die Azure ist eine Reihe von Open Source, Plattform-Befehle für die Arbeit mit Azure Ressourcen. In diesem Artikel werden die verschiedenen Verfahren zum Geben Sie Ihre Kontoanmeldeinformationen Azure-, um Ihr Abonnement Azure Azure CLI Verbindung:

* Führen Sie die `azure login` CLI-Befehl über Azure Active Directory authentifizieren. Diese Methode erhalten Sie zentralen Zugriff auf CLI-Befehle in beiden [Befehlsmodi](#CLI-command-modes). Wenn Sie den Befehl ohne zusätzliche Optionen ausführen `azure login` fordert Sie weiterhin interaktiv über ein Webportal anmelden. Für weitere `azure login` Befehl Optionen, finden Sie unter der Szenarien in diesem Artikel, oder geben Sie `azure login --help`.

* Wenn Sie nur Azure Servicemanagement Modus CLI-Befehle (nicht empfohlen für die meisten neuen Bereitstellungen) verwenden müssen, können Sie herunterladen und installieren eine Einstellungsdatei veröffentlichen auf Ihrem Computer. 

Wenn Sie die CLI bereits installiert haben, finden Sie unter [Installieren der CLI Azure](xplat-cli-install.md). Wenn Sie ein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](http://azure.microsoft.com/free/) nur wenigen Minuten erstellen. 

Hintergrundinformationen zu anderen Konto Identitäten und Azure-Abonnements finden Sie unter [wie Azure Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md).






## <a name="scenario-1-azure-login-with-interactive-login"></a>Szenario 1: Azure melden Sie sich mit interaktiven Anmeldung 

Mit bestimmten Konten, die CLI erfordert, dass Sie zum Ausführen `azure login` und anschließend die Anmeldung mit einem Webbrowser über ein Web-Portal, sogenannte *interaktiv anmelden*. Ein häufiger Grund ist bei einem geschäftlichen oder schulnotizbücher-Konto (auch als ein *Organisations-Konto*bezeichnet), die kombinierte Authentifizierung erforderlich eingerichtet ist. Auch verwenden Sie interaktive Anmeldung mit Ihrem Microsoft-Konto, wenn Sie Ressourcenmanager Modusbefehle verwenden möchten.

Interaktive Anmeldung ist einfach: Typ `azure login` – ohne Optionen – wie im folgenden Beispiel gezeigt:

```
azure login
```                                                                                             

Die Ausgabe wird ungefähr wie folgt vor:

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate. 
```
Kopieren Sie den Code, der Sie in der Ausgabe des Befehls angeboten, und öffnen Sie einen Browser zu http://aka.ms/devicelogin oder einer anderen Seite aus, falls angegeben. (Sie können einen anderen Browser auf demselben Computer oder auf einem anderen Computer oder Gerät öffnen.) Geben Sie den Code, und klicken Sie dann aufgefordert werden, geben Sie den Benutzernamen und das Kennwort für die Identität, die Sie verwenden möchten. Wenn dieser Vorgang abgeschlossen ist, führt die Verwaltungsshell Befehl die Anmeldung an. Es sieht ungefähr wie folgt aus:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK
    
>[AZURE.NOTE]  Mit interaktive Anmeldung werden Authentifizierung und Autorisierung mit durchgeführt Azure Active Directory. Wenn Sie eine Microsoft-Kontoidentität verwenden, greift der Anmeldevorgang Ihrer Azure Active Directory-Standarddomäne auf. (Wenn Sie sich für ein kostenloses Azure-Konto angemeldet, Azure Active Directory automatisch erstellt eine Standarddomäne für Ihr Konto.)

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Szenario 2: Azure melden Sie sich mit einem Benutzernamen und Kennwort


Verwenden Sie die `azure login` -Befehl mit den Benutzernamen (`-u`) Parameter zur Authentifizierung, wenn Sie eine geschäftlichen verwenden möchten oder Schule zu berücksichtigen, die keine kombinierte Authentifizierung erforderlich. Sie werden aufgefordert, in der Befehlszeile für das Kennwort (oder Sie können optional das Kennwort übergeben, als zusätzliche Parameter von der `azure login` Befehl). Im folgende Beispiel wird der Benutzername, der ein Organisations-Konto übergeben:

    azure login -u myUserName@contoso.onmicrosoft.com
    
Klicken Sie dann aufgefordert werden, geben Sie Ihr Kennwort ein:

    info:    Executing command login
    Password: *********
    
Der Anmeldevorgang vervollständigt klicken Sie dann auf.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Ist dies der ersten Mal Protokollierung mit den folgenden Anmeldeinformationen, werden Sie aufgefordert, stellen Sie sicher, dass eine Authentifizierungstoken zwischengespeichert werden soll. Diese Meldung wird auch, wenn Sie bisher verwendet die `azure logout` Befehl (später in diesem Artikel beschrieben). Führen Sie zum Umgehen dieses verwendendes Automatisierungsszenarien ausführen `azure login` mit den `-q` Parameter.

   

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Szenario 3: Azure melden Sie sich mit einem Dienst Tilgungsanteile

Wenn Sie einem Dienst Tilgungsanteile für eine Active Directory-Anwendung erstellen, und die Dienst Tilgungsanteile Berechtigungen für Ihr Abonnement hat, können Sie mithilfe der `azure login` Befehl aus, um die Tilgungsanteile Dienst authentifizieren. Abhängig von Ihrem Szenario, können Sie die Anmeldeinformationen für den Dienst Tilgungsanteile bereitstellen, als explizite Parameter der `azure login` Befehl. Beispielsweise übergibt der folgende Befehl aus der Dienst Benutzerprinzipalnamen und Active Directory-Mandanten-ID:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Anschließend werden Sie aufgefordert, das Kennwort einzugeben. Sie können auch die Anmeldeinformationen durch einen CLI Skript oder einer Anwendung Code, oder verwenden Sie ein Zertifikat mit die Tilgungsanteile Dienst nicht interaktiv für Szenarios für die Automatisierung authentifiziert. Details und Beispielen finden Sie unter [Authentifizierung ein Dienst Tilgungsanteile Azure Ressourcenmanager](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Szenario 4: Verwenden der Einstellungsdatei veröffentlichen

Wenn Sie nur die Azure Servicemanagement Modus CLI-Befehle (z. B. Azure-virtuellen Computern im Bereitstellungsmodell klassischen bereitstellen) verwenden müssen, können Sie eine Verbindung herstellen, mithilfe einer Einstellungsdatei veröffentlichen. Diese Methode installiert ein Zertifikat auf Ihrem lokalen Computer, mit dem Sie Verwaltungsaufgaben für ausführen, solange das Abonnement und das Zertifikat gültig sind. 

* **Herunterladen die Einstellungsdatei veröffentlichen** für Ihr Konto sicherzustellen, dass die im Servicemanagement Modus durch Eingabe ist `azure config mode asm`. Führen Sie dann den folgenden Befehl ein:

        azure account download

Dies wird als Standardbrowser geöffnet und fordert Sie [Azure klassischen Portal](https://manage.windowsazure.com)anmelden. Nachdem Sie sich angemeldet einer `.publishsettings` Dateidownloads. Notieren Sie, wo diese Datei gespeichert ist.

>[AZURE.NOTE] Wenn Ihr Konto mehrere Azure Active Directory-Mandanten zugeordnet ist, können Sie aufgefordert, welche Active Directory wählen Sie eine Einstellungsdatei veröffentlichen zum herunterladen möchten.

Nachdem mit der Downloadseite ausgewählt wurden, oder besuchen Sie das klassische Azure-Portal, wird der ausgewählten Active Directory vom klassischen Portal und dem Download Seite als Standardwert verwendet. Nachdem Sie ein Standardwert eingerichtet wurde, wird den Text "__hier klicken, um zur Auswahlseite zurückzukehren__" am oberen Rand der Seite zum Herunterladen. Verwenden Sie den bereitgestellten Link, um zur Auswahlseite zurückzukehren.

* **So importieren Sie die Einstellungsdatei veröffentlichen**, mit dem folgenden Befehl ausführen:

        azure account import <path to your .publishsettings file>

>[AZURE.IMPORTANT]Nach dem Import der Einstellungen veröffentlichen, löschen Sie die `.publishsettings` Datei. Es ist nicht mehr erforderlich, von der CLI Azure und stellt ein Sicherheitsrisiko dar, wie sie den Zugriff auf Ihr Abonnement verwendet werden kann.

## <a name="cli-command-modes"></a>CLI Befehlsmodi

Die CLI Azure bietet zwei Befehlsmodi für die Arbeit mit Azure Ressourcen, die mit anderen Befehl an:

* **Ressourcenmanager Modus** - für die Arbeit mit Azure Ressourcen im Bereitstellungsmodell Ressourcenmanager. Führen Sie zum Festlegen dieser Modus `azure config mode arm`.

* **Servicemanagement Modus** - für die Arbeit mit Azure Ressourcen im Bereitstellungsmodell klassischen. Führen Sie zum Festlegen dieser Modus `azure config mode asm`.

Beim ersten Mal installiert, wird die aktuelle Version der CLI im Ressourcenmanager Modus.

>[AZURE.NOTE]Die Ressourcenmanager und Servicemanagement Modus schließen sich gegenseitig aus. D. h., werden nicht in einem einzigen Modus erstellte Ressourcen aus dem anderen Modus verwaltet.

## <a name="multiple-subscriptions"></a>Mehrere Abonnements

Wenn Sie mehrere Azure-Abonnements verfügen, gewährt das Herstellen einer Verbindung mit Azure Zugriff auf alle Abonnements zugeordnet sind Ihre Anmeldeinformationen aus. Ein Abonnement ist als Standarddrucker ausgewählt, und von der CLI Azure verwendet, beim Ausführen von Vorgängen. Sie können die Abonnements anzeigen, einschließlich des aktuellen Standard-Abonnements, mithilfe der `azure account list` Befehl. Dieser Befehl gibt Informationen ähnlich wie die folgende:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

Die **aktuellen** Spalte in der vorstehenden Liste zeigt an, dass das aktuelle Standardabonnement als Azure-Sub-1. Verwenden Sie zum Ändern der Standard-Abonnements die `azure account set` Befehl aus, und geben Sie das Abonnement, das, die als Standard verwendet werden soll. Beispiel:

    azure account set Azure-sub-2

Hiermit ändern Sie das standardmäßigen Abonnement Azure-Sub-2.

> [AZURE.NOTE] Ändern des Standard-Abonnements wird sofort wirksam und eine globale Änderung ist; neue Azure CLI Befehle verwenden, ob die Ausführung von der gleichen Befehlszeile Instanz oder einer anderen Instanz, Sie das neue Standardabonnement.

Wenn Sie ein Abonnement nicht standardmäßige mit der CLI Azure verwenden möchten, aber nicht das aktuelle Standard ändern möchten, können Sie mithilfe der `--subscription` option für den Befehl aus, und geben Sie den Namen des Abonnements, die Sie für den Vorgang verwenden möchten.

Nachdem Sie Ihr Abonnement Azure verbunden sind, können Sie beginnen, verwenden die Azure CLI-Befehle zum Arbeiten mit Azure Ressourcen.



## <a name="storage-of-cli-settings"></a>Speicherung CLI-Einstellungen

Ob sich der Benutzer in der `azure login` Befehl oder importieren veröffentlichungseinstellungen, Ihr CLI Profil und die Protokolle gespeicherten einer `.azure` Verzeichnis befindet sich Ihrer `user` Directory. Ihre `user` Verzeichnis von Ihrem Betriebssystem geschützt ist. Es empfiehlt sich jedoch, dass Sie zusätzliche Schritte zum Verschlüsseln Unternehmen Ihre `user` Directory. In der folgenden Methoden können Sie vorgehen:

* Unter Windows ändern Sie die Eigenschaften Directory oder verwenden Sie BitLocker.
* Mac für das Verzeichnis FileVault aktivieren.
* Verwenden Sie auf Ubuntu die Funktion für die verschlüsselte Start Directory. Andere Linux-Versionen bieten vergleichbare Features.

## <a name="logging-out"></a>Abmelden

Zum abmelden möchten, verwenden Sie den folgenden Befehl aus:

    azure logout -u <username>

Wenn die Abonnements mit dem Konto verbunden sind nur mit Active Directory, Abmelden löscht die Abonnementinformationen aus dem lokalen Profil authentifiziert wurden. Jedoch, wenn eine Einstellungsdatei veröffentlichen auch für Abonnements importiert wurde, Abmelden nur Löschvorgänge Active Directory Informationen aus dem lokalen Profil in Bezug.
## <a name="next-steps"></a>Nächste Schritte

* Um Azure CLI-Befehle verwenden zu können, finden Sie unter [Azure CLI-Befehle im Modus Ressourcenmanager](./virtual-machines/azure-cli-arm-commands.md) und [Azure CLI-Befehle im Modus Servicemanagement](virtual-machines-command-line-tools.md).

* Erfahren Sie mehr über die CLI Azure, Quellcode, Melden von Problemen, herunterladen, oder zum Projekt beitragen, finden Sie auf der [GitHub Repository für die CLI Azure](https://github.com/azure/azure-xplat-cli).

* Wenn Sie mit dem Azure CLI oder Azure Probleme auftreten, besuchen Sie die [Foren Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


