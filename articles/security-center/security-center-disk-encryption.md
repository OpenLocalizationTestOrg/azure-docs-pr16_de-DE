<properties
   pageTitle="Verschlüsseln einer Azure-virtuellen Computern | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen, eine Azure-virtuellen Computern nach Erhalt einer Benachrichtigung von Azure-Sicherheitscenter verschlüsseln."
   services="security, security-center"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/27/2016"
   ms.author="tomsh"/>

# <a name="encrypt-an-azure-virtual-machine"></a>Verschlüsseln einer Azure-virtuellen Computern
Azure-Sicherheitscenter wird Sie benachrichtigt, sobald Sie virtuelle Computer verfügen, die nicht verschlüsselt sind. Diese Benachrichtigungen werden als hoher Priorität angezeigt und wird empfohlen, diesen virtuellen Computern verschlüsseln.

![Datenträger Verschlüsselung Empfehlungen](./media/security-center-disk-encryption\security-center-disk-encryption-fig1.png)

> [AZURE.NOTE] Die Informationen in diesem Dokument gelten für die Preview-Version von Azure-Sicherheitscenter.

Azure-virtuellen Computern Verschlüsselung, die vom Sicherheitscenter Azure identifiziert wurden als Verschlüsselung benötigen, empfehlen wir die folgenden Schritte aus:

- Installieren und Konfigurieren von Azure PowerShell. Dadurch können Sie zum Ausführen der PowerShell-Befehle erforderlich, um die erforderliche Azure-virtuellen Computern verschlüsseln einzurichten.
- Beziehen Sie, und führen Sie das Skript Azure Datenträger Verschlüsselung Vorkenntnisse Azure PowerShell
- Verschlüsseln Sie Ihre virtuellen Computern

Das Ziel dieses Dokuments ist, können Sie zum Verschlüsseln der virtuellen Computern, auch wenn Sie mit wenig oder gar Hintergrund in Azure PowerShell haben.
Dieses Dokument wird davon ausgegangen, dass Sie Windows 10 als Clientcomputer verwenden werden, aus der Sie die Verschlüsselung Azure-Festplatten konfigurieren.

Es gibt viele Methoden, mit die die erforderlichen Komponenten einrichten und konfigurieren Sie die Verschlüsselung für Azure virtuellen Computern verwendet werden können. Wenn Sie bereits über Kenntnisse in Azure PowerShell oder Azure CLI sind, müssen Sie vielleicht alternative Ansätze verwenden.

> [AZURE.NOTE] Weitere Informationen zum alternative Ansätze zum Konfigurieren der Verschlüsselung für Azure-virtuellen Computern finden Sie unter [Azure Datenträger Verschlüsselung für Windows und Linux Azure-virtuellen Computern](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

## <a name="install-and-configure-azure-powershell"></a>Installieren und Konfigurieren von Azure PowerShell
Sie benötigen Azure PowerShell Version 1.2.1 oder höher auf Ihrem Computer installiert. Im Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) enthält alle Schritte, die Sie Ihren Computer für die Arbeit mit Azure PowerShell Bereitstellung müssen. Die einfachste besteht darin, die in diesem Artikel genannten Web PI-Installation-Vorgehensweise anzuwenden. Auch wenn Sie bereits Azure PowerShell installiert ist, installieren erneut mit der Web PI Ansatz, sodass Sie die neueste Version von Azure PowerShell haben.


## <a name="obtain-and-run-the-azure-disk-encryption-prerequisites-configuration-script"></a>Beziehen Sie, und führen Sie die erforderlichen Komponenten Azure Datenträger-Verschlüsselung Konfigurationsskript
Alle erforderliche für die Verschlüsselung der Azure-virtuellen Computern wird das Azure Datenträger Verschlüsselung Vorkenntnisse Skript einrichten.

1.  Wechseln Sie zur Seite GitHub, die das [Azure Datenträger Verschlüsselung erforderliche Setup-Skript](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)enthält.
2.  Klicken Sie auf der Seite GibHub auf die Schaltfläche **Rohstoffe** .
3.  Verwenden Sie **STRG + A** zum Auswählen des gesamten Texts auf der Seite, und verwenden Sie dann **STRG + C,** um alle den Text auf der Seite in die Zwischenablage zu kopieren.
4.  Öffnen Sie **Editor** , und fügen Sie den kopierten Text in Editor ein.
5.  Erstellen eines neuen Ordners auf Laufwerk C: mit dem Namen **AzureADEScript**.
6.  Speichern Sie die Datei – klicken Sie auf **Datei**, und klicken Sie auf **Speichern unter**. Klicken Sie im Textfeld den Wert Geben Sie **"ADEPrereqScript.ps1"** , und klicken Sie auf **Speichern**. (Stellen Sie sicher, dass Sie das setzen den Namen in Anführungszeichen ein, andernfalls wird es die Datei speichern, mit der Erweiterung TXT).

Nachdem Sie nun der Skriptinhalt gespeichert wird, öffnen Sie das Skript in der PowerShell ISE aus:

1.  Klicken Sie im Menü Start auf **Cortana**. Bitten Sie durch Eingeben der **PowerShell** in das Suchtextfeld ein Cortana **Cortana** "PowerShell" ein.
2.  Klicken Sie mit der rechten Maustaste auf **Windows PowerShell ISE** , und klicken Sie auf **als Administrator ausführen**.
3.  In den **Administrator: Windows PowerShell ISE** Fenster, klicken Sie auf **Ansicht** , und klicken Sie dann auf **Skriptbereich anzeigen**.
4.  Wenn Sie im Bereich **Befehle** auf der rechten Seite des Fensters angezeigt wird, klicken Sie auf das **"X"** in der oberen rechten Ecke des Bereichs, um es zu schließen. Wenn der Text zu klein, damit Sie sehen, verwenden Sie **STRG + Add** ("Hinzufügen" ist das Vorzeichen "+"). Wenn der Text zu groß ist, verwenden Sie **STRG + subtrahieren** (subtrahieren ist die "-" Anmelden).
5.  Klicken Sie auf **Datei** , und klicken Sie dann auf **Öffnen**. Navigieren Sie zu dem Ordner **C:\AzureADEScript** und die Doppelklicken Sie auf die **ADEPrereqScript**.
6.  Der Inhalt des **ADEPrereqScript** wird jetzt in der PowerShell ISE angezeigt und ist farbcodiert, um Sie der verschiedenen Komponenten, wie Befehle, Parameter und Variablen einfacher finden Sie unter Hilfe.

Es sollte nun ähnlich der folgenden Abbildung angezeigt.

![PowerShell ISE-Fenster](./media/security-center-disk-encryption\security-center-disk-encryption-fig2.png)

Im obere Bereich genannt wird im Bereich"Skript", und klicken Sie im unteren Bereich wird als die "Console" bezeichnet. Wir verwenden diese Ausdrücke weiter unten in diesem Artikel.

## <a name="run-the-azure-disk-encryption-prerequisites-powershell-command"></a>Führen Sie die erforderlichen Komponenten Azure Datenträger-Verschlüsselung PowerShell-Befehl

Das Skript Azure Datenträger Verschlüsselung Voraussetzungen für werden Sie aufgefordert, für die folgenden Informationen, nachdem Sie das Skript zu starten:

- **Gruppe Ressourcennamen** – Name der Ressourcengruppe, die Sie der Taste Tresor in setzen möchten.  Eine neue Ressourcengruppe mit der eingegebene Name wird erstellt, wenn es nicht bereits mit dem Namen erstellt. Wenn Sie bereits eine Ressourcengruppe, die Sie in dieses Abonnement verwenden möchten verfügen, geben Sie dann den Namen der Gruppe.
- **Key Tresor Name** - Namen für die Taste Tresor in welcher Schlüssel für die Verschlüsselung sind, platziert werden soll. Eine neue Schlüssel Tresor mit diesem Namen wird erstellt, wenn Sie bereits eine Taste Tresor mit diesem Namen besitzen. Wenn Sie bereits über eine Taste Tresor, die Sie verwenden möchten haben, geben Sie den Namen der vorhandenen Schlüssel Tresor.
- **Speicherort** - Speicherort, der die wichtigsten Tresor. Stellen Sie sicher, dass die Taste Tresor und virtuellen Computern verschlüsselt werden an derselben Stelle sind. Wenn Sie nicht, dass die Position wissen, gibt es Schritte später in diesem Artikel, die Sie ermitteln, wie angezeigt wird.
- **Name der Azure Active Directory Anwendung** – Name der Azure-Active Directory-Anwendung, die zum Schreiben von geheimen Informationen zum Schlüssel Tresor verwendet werden. Eine neue Anwendung mit diesem Namen wird erstellt, wenn eine nicht vorhanden ist. Wenn Sie bereits eine Azure Active Directory-Anwendung, die Sie verwenden möchten verfügen, geben Sie den Namen der betreffenden Azure Active Directory-Anwendung.

> [AZURE.NOTE] Wenn Sie wissen möchten anderem Ort tätig Gründe für eine Azure Active Directory-Anwendung zu erstellen, melden Sie finden Sie unter *Registrieren einer Anwendung mit Azure Active Directory* Abschnitt in die [Erste Schritte mit Azure Schlüssel Tresor](../key-vault/key-vault-get-started.md)Artikel.

Führen Sie die folgenden Schritte aus, um eine Azure-virtuellen Computern verschlüsseln ein:

1.  Wenn Sie die PowerShell ISE geschlossen haben, öffnen Sie eine Instanz der PowerShell ISE erhöhte. Folgen Sie den Anweisungen in diesem Artikel, wenn der PowerShell ISE noch nicht geöffnet ist. Wenn Sie das Skript geschlossen haben, öffnen Sie die **ADEPrereqScript.ps1** durch Klicken auf **Datei**, klicken Sie dann auf **Öffnen** , und wählen Sie das Skript aus dem Ordner **c:\AzureADEScript** . Wenn Sie in diesem Artikel vom Anfang befolgt haben, einfach fahren Sie mit dem nächsten Schritt fort.
2.  In der Verwaltungskonsole von der PowerShell ISE (im unteren Bereich der PowerShell ISE) ändern Sie den Fokus auf den lokalen des Skripts, indem Sie **cd c:\AzureADEScript** eingeben, und drücken Sie die **EINGABETASTE**.
3.  Legen Sie die Ausführungsrichtlinie auf Ihrem Computer, sodass Sie das Skript ausführen können. Geben Sie in der Verwaltungskonsole **Set-ExecutionPolicy uneingeschränkte** ein, und drücken Sie dann die EINGABETASTE. Wenn ein anderer Benutzer zu den Auswirkungen der Änderung zur Ausführungsrichtlinie das Dialogfeld angezeigt wird, klicken Sie entweder **auf alle Ja** oder **Ja** (Wenn **Ja, alle**, wenn **Ja, alle**, klicken Sie dann auf **Ja,**nicht angezeigt wird, wählen Sie diese Option – angezeigt wird).
4.  Melden Sie sich bei Ihrem Azure-Konto. Klicken Sie in der Verwaltungskonsole **Login-AzureRmAccount** Geben Sie ein, und drücken die **EINGABETASTE**. Das Dialogfeld wird angezeigt, in dem Sie Ihre Anmeldeinformationen eingeben (sicherzustellen, dass Sie die Berechtigung zum Ändern der virtuellen Computer – Wenn Sie nicht berechtigt, Sie ist nicht möglich, dass sie verschlüsselt. Wenn Sie nicht sicher sind, bitten Sie Ihren Abonnementbesitzer oder Administrator). Informationen zu Ihrer **Umgebung**, **Konto**, **TenantId**, **SubscriptionId** und **CurrentStorageAccount**sollte angezeigt werden. Kopieren Sie die **SubscriptionId** in Editor ein. Sie müssen diese in Schritt 6 # verwenden.
5.  Suchen Sie nach welches Abonnement des virtuellen Computers gehört und die Position. Wechseln Sie zur [https://portal.azure.com](ttps://portal.azure.com) , und melden Sie sich an.  Klicken Sie auf der linken Seite der Seite auf **virtuellen Computern**. Sie sehen eine Liste mit Ihren virtuellen Computern und die Abonnements, denen sie angehören.

    ![Virtuellen Computern](./media/security-center-disk-encryption\security-center-disk-encryption-fig3.png)

6.  Kehren Sie zu der PowerShell ISE zurück. Festlegen des Abonnement Kontexts, in dem das Skript ausgeführt wird. **Auswählen-AzureRmSubscription – SubscriptionId < Your_subscription_Id >** (ersetzen **< Your_subscription_Id >** mit der ist-Abonnement-ID) Geben Sie in der Verwaltungskonsole ein, und drücken Sie die **EINGABETASTE**. Informationen zu den-Umgebung, **Konto**, **TenantId**, **SubscriptionId** und **CurrentStorageAccount**werden angezeigt.
7.  Sie können nun zum Ausführen des Skripts. Klicken Sie auf die Schaltfläche **Skript ausführen** , oder drücken Sie **F5** auf der Tastatur.

    ![Ausführen von PowerShell-Skript](./media/security-center-disk-encryption\security-center-disk-encryption-fig4.png)

8.  Das Skript gefragt werden, für **ResourceGroupName:** -Geben Sie den Namen der *Ressourcengruppe* zu verwenden, und klicken Sie dann **die EINGABETASTE**. Wenn Sie eine besitzen, geben Sie einen Namen für eine neue verwendet werden soll. Wenn Sie bereits eine *Ressourcengruppe* , die Sie (z. B. dasjenige Ihres virtuellen Computers haben in) verwenden möchten, geben Sie den Namen der vorhandenen Ressourcengruppe.
9.  Das Skript gefragt werden, für **KeyVaultName:** -Geben Sie den Namen des *Schlüssel Tresor* zu verwenden, und klicken Sie dann die EINGABETASTE. Wenn Sie eine besitzen, geben Sie einen Namen für eine neue verwendet werden soll. Wenn Sie bereits über eine Taste Tresor, die Sie verwenden möchten haben, geben Sie den Namen des vorhandenen *Schlüssel Tresor*.
10. Das Skript fragt **Speicherort:** – Geben Sie den Namen des Speicherorts, in dem sich der virtuellen Computer verschlüsseln soll befindet, und drücken Sie **EINGABETASTE**. Wenn Sie den Speicherort vergessen haben, kehren Sie zu Schritt #5.
11. Das Skript gefragt werden, für **AadAppName:** -Geben Sie den Namen der Anwendung *Azure Active Directory* zu verwenden, und klicken Sie dann **die EINGABETASTE**. Wenn Sie eine besitzen, geben Sie einen Namen für eine neue verwendet werden soll. Wenn Sie bereits eine *Azure Active Directory-Anwendung* , die Sie verwenden möchten verfügen, geben Sie den Namen der vorhandenen *Azure Active Directory-Anwendung*.
12. Ein Protokoll im Dialogfeld angezeigt wird. Geben Sie Ihre Anmeldeinformationen (Ja, Sie einmal angemeldet haben, aber jetzt erneut erledigen müssen).
13. Das Skript ausgeführt wird und nach Abschluss des Vorgangs fordert Sie die Werte der **AadClientID**, **AadClientSecret**, **DiskEncryptionKeyVaultUrl**und **KeyVaultResourceId**kopiert. Kopieren Sie jeder dieser Werte in die Zwischenablage zu, und fügen Sie sie in Editor ein.
14. Kehren Sie zu der PowerShell ISE zurück und platzieren Sie den Cursor am Ende der letzten Zeile, und drücken Sie die **EINGABETASTE**.

Die Ausgabe des Skripts sollte nun wie im folgenden Bildschirm aussehen:

![PowerShell-Ausgabe](./media/security-center-disk-encryption\security-center-disk-encryption-fig5.png)

## <a name="encrypt-the-azure-virtual-machine"></a>Verschlüsseln Sie die Azure-virtuellen Computern

Sie können nun zum Verschlüsseln des virtuellen Computers. Wenn Ihre virtuellen Computern in derselben Ressourcengruppe als Ihrem Tresor-Taste befindet, können Sie im Abschnitt Verschlüsselung Schritte verschieben. Wenn Ihre virtuellen Computern nicht in derselben Ressourcengruppe als Ihrem Tresor Schlüssel ist, müssen Sie in der Konsole in der PowerShell ISE Geben Sie Folgendes:

**$resourceGroupName = < Virtual_Machine_RG >**

Ersetzen Sie **< Virtual_Machine_RG >** durch den Namen der Ressourcengruppe, in dem Ihre virtuellen Computern enthalten sind, der von einem einzelnen Angebot. Drücken Sie dann die **EINGABETASTE**.
Um zu bestätigen, dass der richtige Ressourcengruppe Name eingegeben wurde, geben Sie in der ISE PowerShell-Konsole Folgendes ein:

**$resourceGroupName**

Drücken Sie die **EINGABETASTE**. Den Namen der Ressourcengruppe sollte angezeigt werden, denen in Ihren virtuellen Computern ansässig sind. Beispiel:

![PowerShell-Ausgabe](./media/security-center-disk-encryption\security-center-disk-encryption-fig6.png)

### <a name="encryption-steps"></a>Schritte für Verschlüsselung

Zuerst müssen Sie PowerShell den Namen des virtuellen Computers informieren, die Sie verschlüsseln möchten. Geben Sie in der Verwaltungskonsole:

**$vmName = < Your_vm_name >**

Ersetzen Sie **< Your_vm_name >** durch den Namen Ihrer virtuellen Computer (Stellen Sie sicher, dass der Name von einem einfachen Anführungszeichen umgeben ist), und drücken Sie dann die **EINGABETASTE**.

Um zu bestätigen, dass der Name korrekt eingegeben wurde, geben Sie Folgendes ein:

**$vmName**

Drücken Sie die **EINGABETASTE**. Es sollte den Namen des virtuellen Computers angezeigt, die Sie verschlüsseln möchten. Beispiel:

![PowerShell-Ausgabe](./media/security-center-disk-encryption\security-center-disk-encryption-fig7.png)

Es gibt zwei Möglichkeiten, die Sie den Befehl Verschlüsselung zum Verschlüsseln des virtuellen Computers ausgeführt werden können. Die erste Methode besteht darin, die ISE PowerShell-Konsole geben Sie den folgenden Befehl aus:

~~~
Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId
~~~

Drücken Sie die **EINGABETASTE**, nach dem Sie diesen Befehl eingeben.

Die zweite Methode besteht, klicken Sie im Bereich Skript (im oberen Bereich der PowerShell ISE), und führen Sie einen Bildlauf nach unten bis zum Ende des Skripts. Markieren Sie den Befehl aufgeführten, und klicken Sie dann klicken Sie nach rechts, darauf, und klicken Sie dann klicken Sie auf **Auswahl ausführen** oder drücken Sie **F8** auf der Tastatur.

![PowerShell ISE](./media/security-center-disk-encryption\security-center-disk-encryption-fig8.png)

Unabhängig von der Methode verwenden Sie, ein Dialogfeld darüber informiert, dass 10 bis 15 Minuten, damit dauert nach Abschluss des Vorgangs. Klicken Sie auf **Ja**.

Während der Verschlüsselungsprozess stattfindet, können Sie zurückkehren zur Azure-Portal und der Status des virtuellen Computers angezeigt. Klicken Sie auf der linken Seite der Seite auf **virtuellen Computern**, dann klicken Sie in das Blade **virtuellen Computern** , auf den Namen des virtuellen Computers, den Sie verschlüsseln sind. In das Blade, das angezeigt wird, werden Sie feststellen, dass der **Status** besagt, dass die **Aktualisierung**ist. Hierdurch wird veranschaulicht, dass die Verschlüsselung verarbeitet wird.

![Weitere Details zu den virtuellen Computer](./media/security-center-disk-encryption\security-center-disk-encryption-fig9.png)

Kehren Sie zu der PowerShell ISE zurück. Wenn das Skript abgeschlossen ist, sehen Sie, was in der folgenden Abbildung angezeigt wird.

![PowerShell-Ausgabe](./media/security-center-disk-encryption\security-center-disk-encryption-fig10.png)

Um zu veranschaulichen, dass die virtuellen Computern nun verschlüsselt ist, Azure-Portal zurück, und klicken Sie auf **virtuellen Computern** auf der linken Seite der Seite. Klicken Sie auf den Namen des virtuellen Computers, die Sie verschlüsselt. Klicken Sie in das Blade **Einstellungen** auf **Datenträger**.

![Einstellungsoptionen](./media/security-center-disk-encryption\security-center-disk-encryption-fig11.png)

Klicken Sie auf dem **Datenträger** Blade sehen Sie sich, dass **die Verschlüsselung** **aktiviert**ist.

![Eigenschaften der Datenträger](./media/security-center-disk-encryption\security-center-disk-encryption-fig12.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument gelernt Sie eine Azure-virtuellen Computern verschlüsseln. Wenn Sie weitere Informationen zur Azure-Sicherheitscenter, probieren Sie Folgendes ein:

- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen überwachen.
- [Verwaltung und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) - Informationen zum Verwalten und Beantworten von Sicherheitshinweisen
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog Suchen von Beiträgen zu Azure Sicherheit und Kompatibilität
