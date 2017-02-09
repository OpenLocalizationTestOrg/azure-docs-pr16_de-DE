Einige Pakete möglicherweise nicht installiert, verwenden bei der Ausführung unter Azure Pip.  Es kann einfach sein, dass das Paket nicht auf dem Python Paket Index verfügbar ist.  Könnte ein Compiler erforderlich ist (ein Compiler ist nicht verfügbar, auf dem Computer ausgeführt das Web app in Azure-App-Verwaltungsdienst).

In diesem Abschnitt werden Methoden für den Umgang mit diesem Problem erläutert.

### <a name="request-wheels"></a>Räder anfordern

Wenn die Installation des Pakets einen Compiler erforderlich ist, sollten Sie versuchen, sich an den Besitzer des Pakets zum anfordern, dass Räder für das Paket verfügbar gemacht werden.

Mit der zuletzt verwendete Verfügbarkeit von [Microsoft Visual C++ Compiler für Python 2.7][]ist es einfacher zu Pakete erstellen, die systemeigenen Code für Python 2.7 aufweisen.

### <a name="build-wheels-requires-windows"></a>Erstellen von Räder (erfordert Windows)

Hinweis: Wenn Sie diese Option verwenden möchten, stellen Sie sicher, kompilieren das Paket mit einer Python-Umgebung, die die Plattform/Architektur/Version entspricht, die auf der Web app im App-Verwaltungsdienst Azure verwendet wird (Windows/32-bit/2.7 oder 3.4).

Wenn das Paket nicht installiert werden kann, da es einen Compiler benötigt, können Sie den Compiler auf Ihrem lokalen Computer installieren und Erstellen einer Mausrad für das Paket, das Sie dann in Ihrem Repository enthält.

Mac/Linux-Benutzer: Wenn Sie Zugriff auf einen Windows-Computer besitzen, finden Sie unter [Erstellen eines virtuellen Computern unter Windows][] Anleitung zum Erstellen eines virtuellen Computers auf Azure.  Sie können sie Räder erstellen, fügen sie in das Repository und verwerfen bei Bedarf den virtuellen Computer verwenden. 

Für Python 2.7 können Sie [Microsoft Visual C++ Compiler für Python 2.7][]installieren.

Für Python 3.4 können Sie den [Microsoft Visual C++ 2010 Express][]installieren.

Räder erstellen zu können, benötigen Sie das Mausrad Paket:

    env\scripts\pip install wheel

Verwenden Sie `pip wheel` eine Abhängigkeit kompiliert:

    env\scripts\pip wheel azure==0.8.4

Dies erstellt eine .whl-Datei in den Ordner \wheelhouse.  Hinzufügen der \wheelhouse Ordner und Mausrad Dateien an Ihrem Repository.

Bearbeiten Sie Ihre requirements.txt hinzufügen der `--find-links` Option oben. Dies weist Pip nach einer exakten Übereinstimmung im lokalen Ordner aussehen, bevor Sie auf den Python Paket Index.

    --find-links wheelhouse
    azure==0.8.4

Wenn Sie alle Ihre Abhängigkeiten in den Ordner \wheelhouse, und verwenden überhaupt nicht den Python Paket Index möchten, können Sie erzwingen, dass Pip zum Ignorieren des Indexes Paket durch Hinzufügen von `--no-index` am oberen Rand der requirements.txt.

    --no-index

### <a name="customize-installation"></a>Anpassen der installation

Sie können das Bereitstellungsskript zum Installieren eines Pakets in der virtuellen Umgebung mithilfe einen alternativen Installer, wie einfach anpassen\_installieren.  Finden Sie unter deploy.cmd für ein Beispiel, in dem sich kommentiert wird.  Stellen Sie sicher, dass solche Pakete in requirements.txt, um zu verhindern, dass Pip deren Installation nicht aufgeführt werden.

Das Bereitstellungsskript Folgendes hinzu:

    env\scripts\easy_install somepackage

Möglicherweise auch einfach verwenden\_Installation zu Installation ein Exe-Installationsprogramm (einige sind zip-kompatiblen, also einfach\_Installation unterstützt diese).  Fügen Sie das Installationsprogramm in Ihrem Repository und einfach aufrufen\_installieren, indem Sie den Pfad für die ausführbare Datei.

Das Bereitstellungsskript Folgendes hinzu:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Einschließen der virtuellen Umgebung im Repository (erfordert Windows)

Hinweis: Wenn Sie diese Option verwenden möchten, stellen Sie sicher, eine virtuelle Umgebung verwenden, die die Plattform/Architektur/Version entspricht, die auf der Web app im App-Verwaltungsdienst Azure verwendet wird (Windows/32-bit/2.7 oder 3.4).

Wenn Sie die virtuelle Umgebung im Repository einschließen, können Sie verhindern, dass das Bereitstellungsskript virtuellen Umgebung Management auf Azure durch Erstellen einer leeren Datei ausführen:

    .skipPythonDeployment

Es empfiehlt sich, dass die vorhandene virtuelle Umgebung auf die app, um zu verhindern, dass verbleibende Dateien aus, wenn die virtuelle Umgebung automatisch verwaltet wurde gelöscht.


[Erstellen einer virtuellen Computern unter Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler für Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
