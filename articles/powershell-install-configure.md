<properties
    pageTitle="Informationen zum Installieren und konfigurieren Azure PowerShell"
    description="Informationen Sie zum Installieren und Konfigurieren von Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Informationen zum Installieren und konfigurieren Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Was ist Azure PowerShell?
Azure PowerShell handelt es sich um eine Reihe von Modulen, die Cmdlets zum Verwalten von Azure mit Windows PowerShell bereitstellen. Sie können die Cmdlets verwenden, erstellen, testen, bereitstellen und Verwalten von Lösungen und Diensten, die über die Azure-Plattform übermittelt. In den meisten Fällen können die Cmdlets für dieselben Aufgaben wie das Azure-Portal, z. B. erstellen und Konfigurieren von Cloud-Diensten, virtuelle Computer, virtuelle Netzwerke und Web apps verwendet werden.

## <a name="how-versioning-works"></a>Funktionsweise der versionsverwaltung

Azure PowerShell verwendet semantische Versioning, was bedeutet, dass bei Version A > B-Version, und klicken Sie dann auf A weist die APIs aktuellste Version. Darüber hinaus bedeutet, dass die Änderungen in den Mittelwert Hauptversion eines abgeschnitten werden in eine oder mehrere Cmdlets ändern.  Also beispielsweise Version 1.7.0 ein Update eine wichtige Änderung in Azure PowerShell-Versionen 1.x behoben.

Weitere Informationen zu semantische Versioning Methoden in Azure PowerShell, finden Sie unter der semantische Versioning Spezifikation unter: http://semver.org
 
Wenn Sie die neuesten APIs erhalten möchten, verwenden Sie die Version 2.x. Aber wenn Sie Skripts geschrieben haben gegen Version 1.x und Sie möchten nicht aufnehmen abgeschnitten werden Änderungen in Version 2.x der 2.x [Versionsinformationen](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), und klicken Sie dann sollten Sie 1.7.0 installieren beschrieben.

Ein Versionskonflikt kann auftreten, wenn die neueste Version des Profilmoduls installiert ist, und eine frühere Version von ein Modul, das hängt davon ab, anschließend geladen wird. Die einfachste Methode zur Lösung des Problems ist, die neuesten MSI-Datei zu installieren. Die MSI-Datei bereinigt automatisch ältere Versionen der Module.
 
###<a name="installing-module-versions-side-by-side"></a>Installieren von Modul Versionen von nebeneinander

2.1.0 (und Version 1.2.6 für AzureStack) sind die ersten Modulversionen installiert werden soll, und nebeneinander verwendet. Da Azure PowerShell binäre Module verwendet, müssen Sie ein neues PowerShell-Fenster zu öffnen und **Import-Module** verwenden, um eine bestimmte Version der AzureRM-Cmdlets zu importieren:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Versionen vor 2.1.0 (ohne 1.2.6) nicht funktionieren gut nebeneinander mit anderen Versionen der Azure-PowerShell-Modul. Beim Laden einer früheren Version der PowerShell Azure Module mithilfe des Befehls ähnlich dem Eintrag oben inkompatible Versionen des Moduls **AzureRM.Profile** geladen werden, sodass sich die Cmdlets werden Sie aufgefordert, melden Sie sich bei jedem Ausführen von ein Cmdlet, auch nachdem Sie sich angemeldet haben.

Die einfachste Möglichkeit zum beheben, dass dies ist, installieren die neuesten Azure PowerShell aus der WebPI feed oder MSI-Datei – dies frühere Versionen der Module installiert aus dem Katalog entfernt. 

Beachten Sie, dass sowohl die Azure AzureRM Module gemeinsam, Abhängigkeiten haben, wenn Sie beide Module, wenn eine Aktualisierung verwenden, sollten Sie beide aktualisieren. Frühere Versionen des Moduls den Azure haben das gleiche Problem mit nebeneinander Modul die früheren Versionen des Moduls AzureRM geladen haben.

<a id="Install"></a>
## <a name="step-1-install"></a>Schritt 1: Installieren

Im folgenden werden die beiden Methoden, an denen Sie Azure PowerShell installieren können. Sie können es aus dem Katalog PowerShell oder der WebPI installieren.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Installieren von Azure PowerShell aus dem Katalog PowerShell

Die bevorzugte Methode besteht darin, PowerShell-Katalog verwenden. Sie benötigen das PowerShellGet Modul den PowerShell-Katalog verwenden. Diese Option ist hier verfügbar: [PowerShellGallery.com](https://www.powershellgallery.com/)

Installieren von Azure PowerShell 1.3.0 oder höher aus dem PowerShell-Katalog mit einer erweiterten Windows PowerShell oder PowerShell Integrated Scripting-Umgebung (ISE) auffordern, die mit den folgenden Befehlen:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Weitere Informationen zu diesen Befehlen

- **Installieren-Modul AzureRM** Installationen ein Rollup-Modul für die Ressourcenmanager Azure-Cmdlets. Abhängig von das AzureRM-Modul 
- eine bestimmte Versionsbereich für jedes Ressourcenmanager Azure-Modul. Bereich enthaltene Version wird sichergestellt, dass keine abgeschnitten werden Modul Änderungen bei der Installation von AzureRM Module mit derselben Hauptversion enthalten sein können. Bei der Installation des Moduls AzureRM werden alle Ressourcenmanager Azure-Modul, die zuvor nicht installiert wurde heruntergeladen und installiert aus dem Katalog PowerShell. Weitere Informationen zu den semantische Versioning von Azure PowerShell-Module verwendet finden Sie unter [semver.org](http://semver.org). 
- **Azure Modul installieren** Installationen Azure-Modul. Dieses Modul ist das Modul Servicemanagement aus Azure PowerShell 0.9.x. Keine Änderungen Haupt- und für die vorherige Version des Moduls den Azure austauschbare werden sollte sein.

###<a name="installing-azure-powershell-from-webpi"></a>Installieren von Azure PowerShell von WebPI

Installieren von Azure PowerShell 1.0 und höher von WebPI ist dieselbe wie für 0.9.x. Herunterladen von [Azure PowerShell](http://aka.ms/webpi-azps) , und starten Sie die Installation. Wenn Sie Azure PowerShell haben 0.9.x installiert, Version 0.9.x als Teil der Aktualisierung deinstalliert werden. Wenn Sie von PowerShell-Katalog Azure PowerShell-Module installiert haben, entfernt das Installationsprogramm automatisch die Module vor der Installation, um sicherzustellen, dass eine konsistente Azure PowerShell-Umgebung.

> [AZURE.NOTE] Wenn Sie zuvor Azure Module aus dem Katalog PowerShell installiert haben, wird das Installationsprogramm automatisch diese entfernen. Dadurch wird verhindert, dass Verwirrung, über welche Modulversionen Sie installiert haben und wo sich diese befinden. Katalog der PowerShell-Module werden normalerweise in **%ProgramFiles%\WindowsPowerShell\Modules**installieren. Im Gegensatz dazu wird das Installationsprogramm WebPI Azure Module in *installiert *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Wenn während der Installation ein Fehler auftritt, können Sie manuell die Azure entfernen* Ordner im Ordner **%ProgramFiles%\WindowsPowerShell\Modules** , und führen Sie die Installation erneut aus.

Nach Abschluss der Installation, Ihre ```$env:PSModulePath``` Einstellung sollte die Verzeichnisse mit den Azure-PowerShell-Cmdlets enthalten.

> [AZURE.NOTE] Es ist ein bekanntes Problem mit PowerShell **$env: PSModulePath** , die bei der Installation von WebPI auftreten können. Wenn Ihr Computer einen Neustart aufgrund von System-Updates oder anderen Installationen erfordert, führt dies möglicherweise Updates **$env: PSModulePath** nicht den Pfad eingeben, in dem Azure PowerShell installiert ist. In diesem Fall wird möglicherweise eine Meldung 'Cmdlet nicht erkannt' bei dem Versuch, verwenden Sie nach der Installation oder Aktualisierung Azure PowerShell-Cmdlets angezeigt. In diesem Fall sollte dem Neustart das Problem beheben.

Wenn Sie eine Fehlermeldung wie die folgende beim Laden oder Cmdlets ausführen erhalten:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Dies kann behoben werden, indem Sie den Computer neu zu starten, oder importieren die Cmdlets aus c:\Programme Files\WindowsPowerShell\Modules\Azure\XXXX\ wie folgt (, wobei XXXX die Version von PowerShell installiert ist:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Schritt 2: Starten
Sie können die Cmdlets aus standard Windows PowerShell-Konsole oder von PowerShell Integrated Scripting-Umgebung (ISE) ausführen.
Die Methode, die Sie zum Öffnen eines Console verwenden hängt von der Version von Windows, die Sie ausführen:

- Auf einem Computer mit mindestens Windows 8 oder Windows Server 2012, können Sie die integrierte Suche. Aus **den Startbildschirm** die zu formatierenden Power. Dies gibt eine Suchbegriffs Liste der apps, die Windows PowerShell enthält. Klicken Sie zum Öffnen der Verwaltungskonsole auf entweder app. (Die app zum **Startbildschirm** anheften, mit der rechten Maustaste des Symbol.)

- Verwenden Sie das **Startmenü**auf einem Computer mit einer Version vor Windows 8 oder Windows Server 2012. Klicken Sie auf **Alle Programme**, klicken Sie auf **Zubehör**, klicken Sie auf der **Windows PowerShell** -Ordner, und klicken Sie dann auf **Windows PowerShell**, über **das Startmenü** .

Sie können auch Ausführen der **Windows PowerShell ISE** um Menüelemente und Tastenkombinationen verwenden, viele der Aufgaben ausführen möchten, die Sie in der Windows PowerShell-Konsole ausführen möchten. Um die ISE, in der Windows PowerShell-Konsole Cmd.exe, oder in das Feld **Ausführen** verwenden, geben Sie, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Befehle, die Ihnen helfen, erste Schritte

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Schritt 3: Verbinden
Die Cmdlets benötigen Sie Ihr Abonnement, damit sie Ihre Dienste verwalten können. Wenn Sie eine bereits besitzen, können Sie ein Azure-Abonnement kaufen. Anweisungen finden Sie unter [Azure erwerben](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Typ **Login-AzureRmAccount**

2. Geben Sie die e-Mail-Adresse und Ihr Kennwort ein, der mit Ihrem Konto verbunden ist. Azure authentifiziert speichert Anmeldeinformationen für die, und klicken Sie dann das Fenster wird geschlossen.

– ODER –

Melden Sie sich bei der Arbeit oder Schule Konto:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Wenn Sie mehr als einen Mandanten zugeordnet Organisations-Konto verfügen, geben Sie den TenantId Parameter:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Diese nicht interaktiven Log in Methode funktioniert nur mit einem Konto geschäftlichen oder schulnotizbücher. Ein Konto geschäftlichen oder schulnotizbücher ist ein Benutzer, der Ihre Arbeit oder Schule werden verwaltet und in der Azure-Active Directory-Instanz für Ihre Arbeit oder Schule definiert ist. Wenn Sie aktuell kein Konto geschäftlichen oder schulnotizbücher und zum Anmelden bei Ihrem Abonnement Azure ein Microsoft-Konto verwenden, können Sie einfach eine mithilfe der folgenden Schritte erstellen.

> 1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com), und klicken Sie auf **Active Directory**.

> 2. Wenn kein Verzeichnis vorhanden ist, wählen Sie **das Verzeichnis erstellen** aus, und geben Sie die angeforderte Informationen.

> 3. Wählen Sie Ihr Verzeichnis aus, und fügen Sie einen neuen Benutzer. Diese neue Benutzer kann sich mit einem geschäftlichen oder schulnotizbücher-Konto anmelden. Während der Erstellung des Benutzers werden beide eine e-Mail-Adresse für den Benutzer und ein temporäres Kennwort angegeben werden. Speichern Sie diese Informationen, wie sie in Schritt 5 unten verwendet wird.

> 4. Klicken Sie im Portal Azure klassischen wählen Sie **Einstellungen** aus, und wählen Sie dann auf **Administratoren**. Wählen Sie **Hinzufügen**aus, und fügen Sie den neuen Benutzer als Co-Administrator. Dadurch wird das Konto geschäftlichen oder schulnotizbücher, um Ihre Azure-Abonnement zu verwalten.

> 5. Schließlich melden Sie sich aus der Azure klassischen Portal und melden Sie sich dann wieder in die Arbeit mit oder Schule Konto. Wenn dies das erste Mal Protokollierung mit diesem Konto ist, werden Sie aufgefordert, das Kennwort ändern.

> Weitere Informationen zu einem geschäftlichen oder schulnotizbücher-Konto für Microsoft Azure anmelden finden Sie unter [Anmelden für Microsoft Azure als einer Organisation](./active-directory/sign-up-organization.md).

> Weitere Informationen zur Verwaltung von Anmelde- und Abonnement in Azure finden Sie unter [Verwalten von Konten, Abonnements, und Administrative Rollen](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Anzeigen von Details zu Konto und Ihr Abonnement

Sie können mehrere Konten und Abonnements zur Verfügung, für die Verwendung durch Azure PowerShell haben. Sie können mehrere Konten hinzufügen, indem Sie die **Hinzufügen-AzureRmAccount** nur einmal ausgeführt.

Geben Sie **Get-AzureAccount**aus, um die verfügbaren Azure Konten anzuzeigen.

Geben Sie zum Anzeigen Ihrer Abonnements Azure **Get-AzureRmSubscription**aus.

##<a name="a-idhelpagetting-help"></a><a id="Help"></a>Aufrufen der Hilfe##

Diese Ressourcen bieten Hilfe für bestimmte Cmdlets:


-   Innerhalb der Verwaltungskonsole, können Sie das integrierte Hilfesystem verwenden. Das **Hilfe** Cmdlet können Sie auf dieses System. 

- Probieren Sie diese beliebten Foren Hilfe aus der Community:

 - [Azure-Forum im MSDN]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Weitere Informationen


Finden Sie weitere Informationen zur Verwendung der Cmdlets die folgenden Ressourcen:

Grundlegende Anweisungen zur Verwendung von Windows PowerShell finden Sie unter [Verwenden von Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Referenzinformationen zu den-Cmdlets finden Sie unter [Azure Cmdlet verweisen](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Beispiel für Skripts und Anweisungen, um zum Verwalten von Azure verwenden Skripting vertraut zu machen finden Sie im [Script Center](http://go.microsoft.com/fwlink/p/?LinkId=321940).

