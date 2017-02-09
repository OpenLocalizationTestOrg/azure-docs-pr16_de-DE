Das Bereitstellungsskript wird auf Azure Erstellung der virtuellen Umgebung überspringen, wenn das Programm erkennt, dass eine kompatible virtuelle Umgebung bereits vorhanden ist.  Dies kann Bereitstellung erheblich beschleunigen.  Pakete, die bereits installiert sind, werden von Pip übersprungen werden.

In einigen Fällen möchten Sie möglicherweise erzwingen, dass diese virtuelle Umgebung löschen.  Sie erhalten vorgehen, wenn Sie sich entscheiden, eine virtuelle Umgebung als Teil Ihrer Repository aufnehmen möchten.  Sie möchten möglicherweise auch vorgehen, wenn Sie Fluggesellschaften bestimmte Pakete oder Testen von Änderungen an requirements.txt müssen.

Es gibt einige Optionen, um die vorhandene virtuelle Umgebung auf Azure verwalten:

### <a name="option-1-use-ftp"></a>Option 1: Verwenden Sie FTP.

Mit einem FTP-Client eine Verbindung mit dem Server herstellen und So löschen Sie den Ordner Env können zwar.  Beachten Sie, dass einige FTP-Clients (z. B. Webbrowser) wird nicht ermöglichen es Ihnen, Ordner löschen, sodass Sie sicherstellen, dass einen FTP-Client für diese Funktion verwenden möchten, werden möglicherweise schreibgeschützt.  Die FTP-Hostname und Benutzer werden in der Web-app Blade im [Portal Azure](https://portal.azure.com)angezeigt.

### <a name="option-2-toggle-runtime"></a>Option 2: Umschalten Laufzeit

Hier ist eine Alternative, die die Fakultät nutzt, dass das Bereitstellungsskript im Ordner Env gelöscht werden, wenn sie die gewünschte Version der Python übereinstimmt.  Effektiv wird die vorhandene Umgebung löschen, und Erstellen eines neuen Kontos.

1. Wechseln Sie zu einer anderen Version von Python (über runtime.txt oder **Anwendungseinstellungen** vorher in Azure-Portal)
1. Git Pushbenachrichtigungen bei einigen Änderungen (Pip installieren Fehler ignorieren, falls vorhanden)
1. Wechseln Sie zurück zur ersten Version von Python
1. Git bei einigen Änderungen erneut Pushbenachrichtigungen

### <a name="option-3-customize-deployment-script"></a>Option 3: Passen Sie Bereitstellungsskript an

Wenn Sie das Bereitstellungsskript angepasst haben, können Sie den Code in deploy.cmd Löschen des Ordners Env erzwingen ändern.
