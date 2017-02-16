

Wenn Sie ein Projekt der Web-Anwendung für Azure erstellen, können Sie einen virtuellen Computer in Azure bereitstellen. Sie können dann Konfigurieren des virtuellen Computers mit zusätzliche Software oder des virtuellen Computers zu Diagnose- und Debuggen Zwecken verwenden.

Zum Erstellen eines virtuellen Computers beim Erstellen einer Web-Anwendungs, gehen Sie folgendermaßen vor:

1. Klicken Sie auf **Datei**, klicken Sie in Visual Studio > **neu** > **Project** > **Web**, und wählen Sie dann **ASP.NET Web-Anwendung** (unter den **Visual C#-** oder **Visual Basic** -Knoten).
2. Wählen Sie im Dialogfeld **Neues Projekt von ASP.NET** den Typ des Web-Anwendung, die Sie möchten, und im Abschnitt Azure des Dialogfelds (in der unteren rechten Ecke) stellen sicher, dass das Kontrollkästchen **in der Cloud zu hosten** aktiviert ist (dieses Kontrollkästchen ist mit der Bezeichnung **Erstellen remote Ressourcen** in einigen Installationen).

    ![][0]

3. In diesem Beispiel, in der Dropdown-Liste unter Microsoft Azure **virtuellen Computern (Version 1)**wählen Sie aus, und klicken Sie dann auf die Schaltfläche **OK** .
4. Melden Sie sich bei Azure, wenn Sie aufgefordert werden. Das Dialogfeld **Erstellen von virtuellen Computern** angezeigt wird.

    ![][2]

5. Geben Sie im **DNS-Name** einen Namen für den virtuellen Computer aus. Der DNS-Name muss in Azure eindeutig sein. Wenn der eingegebene Name nicht verfügbar ist, wird ein rotes Ausrufezeichen angezeigt.
6. Wählen Sie das Bild, dem Sie auf der Grundlage des virtuellen Computers erstellen möchten, in **der Bildliste** . Sie können auswählen, die Bilder standard Azure-virtuellen Computern oder das Bild, das Sie in Azure hochgeladen haben.
7. Lassen Sie das Kontrollkästchen **Aktivieren IIS und Bereitstellen von Web** aktiviert, sofern Sie einen anderen Webserver installieren möchten. Sie können nicht mehr in Visual Studio zu veröffentlichen, wenn Sie Web bereitstellen deaktivieren. Sie können IIS und Web Bereitstellen eines der eingesetzten Windows Server Bilder, einschließlich Ihrer eigenen benutzerdefinierten Bilder hinzufügen.
8. Wählen Sie in der Liste **Größe** die Größe des virtuellen Computers aus.
9. Geben Sie die Anmeldeinformationen für diesen virtuellen Computer an. Notieren Sie diese, da sie auf dem Computer über Remote Desktop zu benötigen.
10. Wählen Sie in der Liste **Speicherort** der Region des virtuellen Computers zu hosten.
11. Klicken Sie auf die Schaltfläche **OK** , um mit dem Erstellen des virtuellen Computers beginnen. Sie können den Status des Vorgangs im **Ausgabefenster** folgen.

    ![][3]

12. Wenn des virtuellen Computers bereitgestellt wird, werden in einem **PublishScripts** -Knoten in Ihre Lösung veröffentlichten Skripts erstellt. Das Skript veröffentlichte ausgeführt wird und Vorschriften ein virtuellen Computers in Azure. **Das Ausgabefenster** zeigt den Status an. Das Skript führt zum Einrichten des virtuellen Computers die folgenden Aktionen aus:

    * Erstellt des virtuellen Computers an, wenn es nicht bereits vorhanden ist.
    * Erstellt ein Speicherkonto mit einem Namen, der mit beginnt `devtest`, jedoch nur, wenn es noch nicht vorhanden solche Speicher-Konto in der angegebenen Region ist.
    * Erstellt einen Clouddienst als Container des virtuellen Computers und eine Webrolle für die Webanwendung erstellt.
    * Konfiguriert die Web Bereitstellen des virtuellen Computers.
    * IIS und ASP.NET konfiguriert des virtuellen Computers.

    ![][4]

13. (Optional) Sie können mit den neuen virtuellen Computer herstellen. Klicken Sie im **Server-Explorer**erweitern Sie den Knoten **virtuellen Computern** , wählen Sie den Knoten für den virtuellen Computern, die, den Sie erstellt haben, und wählen Sie im angezeigten Kontextmenü, **Herstellen einer Verbindung mit Remotedesktop**. Alternativ können Sie in der **Cloud Explorer** wählen Sie im Kontextmenü auf **Öffnen im Portal** aus und Verbinden mit es des virtuellen Computers.

 ![][5]


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die veröffentlichten Skripts anpassen möchten, die Sie erstellt haben, die lesen Sie detailliertere Informationen [Mithilfe von Windows PowerShell-Skripts zu veröffentlichen, um die Entwicklung und Test-Umgebungen](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
