<properties
    pageTitle="Erstellen von Web-apps mit Django in Azure"
    description="Ein Lernprogramm, die Sie erhalten eine Einführung in die Ausführung einer Python Web app in Azure App Dienst Web Apps."
    services="app-service\web"
    documentationCenter="python"
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-django-in-azure"></a>Erstellen von Web-apps mit Django in Azure

In diesem Lernprogramm beschrieben, wie Schritte Python [Azure App Dienst Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)ausgeführt wird. Web Apps stellt eingeschränkte kostenlosen Hostinganbieter und schnelle Bereitstellung und Python können! Wenn Ihre app wächst, können Sie in bezahlte Hostinganbieter wechseln, und Sie können auch in alle anderen Azure Dienste integrieren.

Erstellen Sie eine Anwendung, die mit Django Web Framework (Siehe alternative Versionen dieses Lernprogramms [wird](web-sites-python-create-deploy-flask-app.md) und [Flaschen](web-sites-python-create-deploy-bottle-app.md)). Sie erstellen die Web-app aus dem Azure Marketplace, Git Bereitstellung einrichten und Repository lokal klonen. Dann führen Sie die Anwendung lokal, vorzunehmen, commit und drücken Sie sie in Azure. Das Lernprogramm wird gezeigt, wie Windows oder Mac/Linux dazu.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.


## <a name="prerequisites"></a>Erforderliche Komponenten

- Windows, Mac oder Linux
- Python 2.7 oder 3.4
- Setuptools, Pip, Virtualenv (nur Python 2.7)
- Git
- [Python-Tools für Visual Studio][] (PTVS) – Hinweis: Dies ist optional

**Hinweis**: TFS Veröffentlichung wird für Projekte, Python derzeit nicht unterstützt.

### <a name="windows"></a>Windows

Wenn Sie bereits Python 2.7 oder 3,4 installierten (32-Bit) besitzen, wird empfohlen, Installation von [Azure SDK für Python 2.7] oder [Azure SDK für Python 3.4] Web Platform Installer verwenden. Dadurch wird die 32-Bit-Version von Python, Setuptools, Pip, Virtualenv usw. (32-Bit-Python ist, was auf den Hostcomputern Azure installiert ist) installiert. Alternativ können Sie Python von [python.org]erhalten.

Wir empfehlen Git [Git für Windows] oder [GitHub für Windows]. Wenn Sie Visual Studio verwenden, können Sie die integrierte Git Unterstützung.

Es empfiehlt sich, [Python Tools 2.2 für Visual Studio]installieren. Dies ist optional, aber wenn Sie [Visual Studio], einschließlich der kostenlosen Visual Studio Community 2013 oder Visual Studio Express 2013 für Web, haben dann dieser Option erhalten Sie eine gute Python IDE.

### <a name="maclinux"></a>Mac/Linux

Sie sollten Python und Git bereits installiert haben, aber Sie können sicherstellen, dass Sie entweder Python 2.7 oder 3.4 haben.


## <a name="web-app-creation-on-portal"></a>Erstellung von Web App-Portal

Der erste Schritt beim Erstellen der app besteht im Erstellen des Web-app über das [Azure-Portal](https://portal.azure.com).

1. Melden Sie sich bei der Azure-Portal an, und klicken Sie auf die Schaltfläche ' **neu** ' in der unteren linken Ecke.
3. Geben Sie im Suchfeld "Python" ein.
4. Klicken Sie in den Suchergebnissen wählen Sie **Django** (durch PTVS veröffentlicht), und klicken Sie auf **Erstellen**.
5. Konfigurieren Sie die neue Django-app, z. B. das Erstellen einer neuen App-Serviceplan und eine neue Ressourcengruppe dafür ein. Klicken Sie dann auf **Erstellen**.
6. Konfigurieren Sie anhand der Anweisungen bei der [Lokalen Bereitstellung von Git Azure App Dienst](app-service-deploy-local-git.md)Git Veröffentlichung für Ihre neu erstellten Web app.

## <a name="application-overview"></a>Übersicht über die Anwendung

### <a name="git-repository-contents"></a>Git Repository Inhalt

Hier ist eine Übersicht über die Dateien, die Sie in die erste Git Repository erledigen müssen, die wir im nächsten Abschnitt Klonen wird.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Hauptfenster Quellen für die Anwendung. Umfasst 3 Seiten (Index zu Kontakt) mit einer Masterlayout. Statischen Inhalt und Skripts einschließen Bootstrap, Jquery, Modernizr und Antworten.

    \manage.py

Lokale Verwaltung und Entwicklung Serversupport. Verwenden Sie diese Option, führen Sie die Anwendung lokal, die Datenbank usw. synchronisieren.

    \db.sqlite3

Standarddatenbank. Enthält die erforderlichen Tabellen für die Anwendung ausgeführt wird, aber keine Benutzer (synchronisieren Sie die Datenbank zum Erstellen eines Benutzers) enthält.

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Project-Dateien für die Verwendung mit [Python-Tools für Visual Studio].

    \ptvs_virtualenv_proxy.py

IIS-Proxy für virtueller Umgebungen und PTVS unterstützt das Debuggen Remoteprozeduraufruf.

    \requirements.txt

Externe Pakete von dieser Anwendung erforderlich. Das Bereitstellungsskript pip wird der in dieser Datei aufgelisteten Pakete installieren.

    \web.2.7.config
    \web.3.4.config

IIS-Konfigurations-Dateien. Das Bereitstellungsskript wird der entsprechende web.x.y.config verwenden, und kopieren sie als web.config.

### <a name="optional-files---customizing-deployment"></a>Optionale Dateien - Bereitstellung anpassen

[AZURE.INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Optionale Dateien - Python Laufzeit

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Weitere Dateien auf dem server

Einige Dateien auf dem Server vorhanden, jedoch Git Repository nicht hinzugefügt werden. Diese werden durch das Bereitstellungsskript erstellt.

    \web.config

IIS-Konfigurationsdatei. Erstellt von web.x.y.config auf jede Bereitstellung.

    \env\

Virtuelle Python-Umgebung. Während der Bereitstellung erstellt, wenn eine kompatible virtuelle Umgebung Web-App nicht bereits vorhanden ist. In requirements.txt aufgelisteten Pakete sind Pip installiert, aber Pip überspringt Installation, wenn die Pakete bereits installiert sind.

In den nächsten 3 Abschnitten beschrieben, wie die Web app-Entwicklung unter 3 verschiedenen Umgebungen fortzusetzen:

- Windows mit Python-Tools für Visual Studio
- Windows mit Befehlszeile
- Mac/Linux mit Befehlszeile


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Web app-Entwicklung Python - Windows - Tools für Visual Studio

### <a name="clone-the-repository"></a>Das Repository Klonen

Zunächst Klonen Sie Repository mithilfe des URLs auf das Portal Azure ein. Weitere Informationen finden Sie unter [Lokale Git Bereitstellung Azure-App-Dienst](app-service-deploy-local-git.md).

Öffnen Sie die Lösungsdatei (.sln), die im Stammverzeichnis des Repositorys enthalten ist.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Erstellen von virtuellen Umgebung

Erstellen Sie nun eine virtuelle Umgebung für lokale Entwicklung. Mit der rechten Maustaste auf **Python Umgebungen** select **Virtuellen Umgebung hinzufügen**.

- Stellen Sie sicher, dass der Name der Umgebung `env`.

- Wählen Sie die Basis Interpreter aus. Vergewissern Sie sich, dieselbe Version von Python ausgewählt ist, die für Ihre Web app (in runtime.txt oder **Anwendungseinstellungen** Falz Web app im Portal Azure) verwenden.

- Stellen Sie sicher, dass die Option zum Herunterladen und Installieren von Paketen aktiviert ist.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Klicken Sie auf **Erstellen**. Dies wird die virtuelle Umgebung erstellen und Installieren von Abhängigkeiten requirements.txt aufgeführt.

### <a name="create-a-superuser"></a>Erstellen Sie ein Hauptbenutzer

Die Datenbank, die mit der Anwendung bereitgestellt hat keine Hauptbenutzer definiert. Um die Anmeldung-Funktionalität in der Anwendung oder die Django Administrator-Benutzeroberfläche (sofern Sie sie aktivieren möchten) zu verwenden, müssen Sie ein Hauptbenutzer zu erstellen.

Führen Sie diese über die Befehlszeile von Ihrem Projektordner:

    env\scripts\python manage.py createsuperuser

Folgen Sie den Anweisungen zum Festlegen der Benutzername, das Kennwort usw. ein.

### <a name="run-using-development-server"></a>Mit Development Server ausgeführt

Drücken Sie F5 für das Debuggen und Ihrem Webbrowser werden zu der Seite, die lokal ausgeführt automatisch geöffnet.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Sie können festlegen, dass Haltepunkte in Quellen, verwenden Sie das Überwachungsfenster usw.. Finden Sie die [Python-Tools für Visual Studio-Dokumentation] für Weitere Informationen zu den verschiedenen Features aus.

### <a name="make-changes"></a>Nehmen Sie Änderungen

Sie können nun durch Ändern der Anwendung Quellen und/oder Vorlagen experimentieren.

Nachdem Sie Ihre Änderungen getestet haben, werden übernehmen Sie diese im Repository Git:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>Installieren Sie weitere Pakete

Die Anwendung möglicherweise Abhängigkeiten jenseits Python und Django verfügen.

Sie können zusätzliche Pakete mit Pip installieren. Klicken Sie zum Installieren eines Pakets mit der rechten Maustaste auf die virtuelle Umgebung, und wählen Sie **Python-Paket installieren**.

Geben Sie zum Installieren der Azure SDK für Python, denen Sie Zugriff auf Azure-Speicher, Dienstbus und andere Dienste Azure bereitstellt, zum Beispiel `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Mit der rechten Maustaste auf die virtuelle Umgebung, und wählen Sie **generieren requirements.txt** requirements.txt aktualisieren.

Übernehmen Sie die Änderungen zu requirements.txt an der Git Repository.

### <a name="deploy-to-azure"></a>Bereitstellen für Azure

Wenn Sie eine Bereitstellung ausgelöst wird, klicken Sie auf **Synchronisieren** , oder **Drücken Sie**. Synchronisieren unterstützt beiden einer Pushbenachrichtigungen, und eine ziehen.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

Die erste Bereitstellung dauert einige Zeit, während der Erstellung wird eine virtuelle Umgebung, Installationspakete usw..

Visual Studio zeigt den Fortschritt der Bereitstellung nicht an. Wenn Sie die Ausgabe überprüfen möchten, finden Sie im Abschnitt [Problembehandlung - Bereitstellung](#troubleshooting-deployment).

Navigieren Sie zu der Azure-URL, die Änderungen anzuzeigen.


## <a name="web-app-development---windows---command-line"></a>Web app-Entwicklung – Windows - Befehlszeile

### <a name="clone-the-repository"></a>Das Repository Klonen

Zunächst Repository mithilfe des URLs auf das Portal Azure klonen und Azure Repository als eine Remote hinzufügen. Weitere Informationen finden Sie unter [Lokale Git Bereitstellung Azure-App-Dienst](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Erstellen von virtuellen Umgebung

Wir erstelle eine neue virtuelle Umgebung hinsichtlich der Entwicklung (kann nicht es Repository hinzufügen). Virtuelle Umgebungen in Python sind nicht verschiebbaren, damit jeder Entwickler arbeiten, klicken Sie auf die Anwendung lokal Erstellen eigener wird.

Vergewissern Sie sich, dieselbe Version von Python ausgewählt ist, die für Ihre Web app (in runtime.txt oder Anwendungseinstellungen Falz Web app im Portal Azure) verwenden.

Für Python 2.7:

    c:\python27\python.exe -m virtualenv env

Für Python 3.4:

    c:\python34\python.exe -m venv env

Installieren von Ihrer Anwendung benötigten externe Pakete. Die Datei requirements.txt können im Stammverzeichnis der Repository Sie um die Pakete in der virtuellen Umgebung zu installieren:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Erstellen Sie ein Hauptbenutzer

Die Datenbank, die mit der Anwendung bereitgestellt hat keine Hauptbenutzer definiert. Um die Anmeldung-Funktionalität in der Anwendung oder die Django Administrator-Benutzeroberfläche (sofern Sie sie aktivieren möchten) zu verwenden, müssen Sie ein Hauptbenutzer zu erstellen.

Führen Sie diese über die Befehlszeile von Ihrem Projektordner:

    env\scripts\python manage.py createsuperuser

Folgen Sie den Anweisungen zum Festlegen der Benutzername, das Kennwort usw. ein.

### <a name="run-using-development-server"></a>Mit Development Server ausgeführt

Starten Sie die Anwendung unter einer Entwicklungsserver mit den folgenden Befehl aus:

    env\scripts\python manage.py runserver

Die Konsole wird die URL angezeigt und Port Server überwacht:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Öffnen Sie dann Ihren Webbrowser die URL ein.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Nehmen Sie Änderungen

Sie können nun durch Ändern der Anwendung Quellen und/oder Vorlagen experimentieren.

Nachdem Sie Ihre Änderungen getestet haben, werden übernehmen Sie diese im Repository Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installieren Sie weitere Pakete

Die Anwendung möglicherweise Abhängigkeiten jenseits Python und Django verfügen.

Sie können zusätzliche Pakete mit Pip installieren. Geben Sie beispielsweise zum Installieren Azure SDK für Python, denen Sie Zugriff auf Azure-Speicher, Dienstbus und andere Dienste Azure bereitstellt:

    env\scripts\pip install azure

Vergewissern Sie sich requirements.txt zu aktualisieren:

    env\scripts\pip freeze > requirements.txt

Bestätigen Sie die Änderungen an:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Bereitstellen für Azure

Um eine Bereitstellung ausgelöst wird, drücken Sie die Änderungen vor, um Azure:

    git push azure master

Die Ausgabe der Bereitstellungsskript, einschließlich der Erstellung virtueller Umgebung, Installation von Paketen, Erstellung von web.config wird angezeigt.

Navigieren Sie zu der Azure-URL, die Änderungen anzuzeigen.


## <a name="web-app-development---maclinux---command-line"></a>Web app-Entwicklung - Mac/Linux - Befehlszeile

### <a name="clone-the-repository"></a>Das Repository Klonen

Zunächst Repository mithilfe des URLs auf das Portal Azure klonen und Azure Repository als eine Remote hinzufügen. Weitere Informationen finden Sie unter [Lokale Git Bereitstellung Azure-App-Dienst](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Erstellen von virtuellen Umgebung

Wir erstelle eine neue virtuelle Umgebung hinsichtlich der Entwicklung (kann nicht es Repository hinzufügen). Virtuelle Umgebungen in Python sind nicht verschiebbaren, damit jeder Entwickler arbeiten, klicken Sie auf die Anwendung lokal Erstellen eigener wird.

Vergewissern Sie sich, dieselbe Version von Python ausgewählt ist, die für Ihre Web app (in runtime.txt oder Anwendungseinstellungen Falz Web app im Portal Azure) verwenden.

Für Python 2.7:

    python -m virtualenv env

Für Python 3.4:

    python -m venv env

oder

    pyvenv env

Installieren von Ihrer Anwendung benötigten externe Pakete. Die Datei requirements.txt können im Stammverzeichnis der Repository Sie um die Pakete in der virtuellen Umgebung zu installieren:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Erstellen Sie ein Hauptbenutzer

Die Datenbank, die mit der Anwendung bereitgestellt hat keine Hauptbenutzer definiert. Um die Anmeldung-Funktionalität in der Anwendung oder die Django Administrator-Benutzeroberfläche (sofern Sie sie aktivieren möchten) zu verwenden, müssen Sie ein Hauptbenutzer zu erstellen.

Führen Sie diese über die Befehlszeile von Ihrem Projektordner:

    env/bin/python manage.py createsuperuser

Folgen Sie den Anweisungen zum Festlegen der Benutzername, das Kennwort usw. ein.

### <a name="run-using-development-server"></a>Mit Development Server ausgeführt

Starten Sie die Anwendung unter einer Entwicklungsserver mit den folgenden Befehl aus:

    env/bin/python manage.py runserver

Die Konsole wird die URL angezeigt und Port Server überwacht:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Öffnen Sie dann Ihren Webbrowser die URL ein.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Nehmen Sie Änderungen

Sie können nun durch Ändern der Anwendung Quellen und/oder Vorlagen experimentieren.

Nachdem Sie Ihre Änderungen getestet haben, werden übernehmen Sie diese im Repository Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installieren Sie weitere Pakete

Die Anwendung möglicherweise Abhängigkeiten jenseits Python und Django verfügen.

Sie können zusätzliche Pakete mit Pip installieren. Geben Sie beispielsweise zum Installieren Azure SDK für Python, denen Sie Zugriff auf Azure-Speicher, Dienstbus und andere Dienste Azure bereitstellt:

    env/bin/pip install azure

Vergewissern Sie sich requirements.txt zu aktualisieren:

    env/bin/pip freeze > requirements.txt

Bestätigen Sie die Änderungen an:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Bereitstellen für Azure

Um eine Bereitstellung ausgelöst wird, drücken Sie die Änderungen vor, um Azure:

    git push azure master

Die Ausgabe der Bereitstellungsskript, einschließlich der Erstellung virtueller Umgebung, Installation von Paketen, Erstellung von web.config wird angezeigt.

Navigieren Sie zu der Azure-URL, die Änderungen anzuzeigen.


## <a name="troubleshooting---package-installation"></a>Problembehandlung - Paketinstallation

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Problembehandlung - virtuellen Umgebung

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="troubleshooting---static-files"></a>Problembehandlung - statische Dateien

Django weist des Konzepts der statische Dateien sammeln. Dadurch gelangen alle statischen Dateien aus dem ursprünglichen Speicherort und kopiert sie in einem einzelnen Ordner. Für diese Anwendung, werden sie in kopiert `/static`.

Dies geschieht, da statische Dateien von verschiedenen Django 'apps' stammen können. Beispielsweise befinden sich die statischen Dateien aus der Django Admin-Schnittstellen in einem Unterordner Django Bibliothek, in der virtuellen Umgebung. Statische Dateien, die von dieser Anwendung definiert befinden sich `/app/static`. Während Sie weitere Django 'apps' verwenden, müssen Sie statische Dateien, die an mehreren Speicherorten gespeichert.

Wenn die Anwendung im Debuggen-Modus ausgeführt, dient die Anwendung statischen Dateien aus dem ursprünglichen Speicherort aus.

Wenn die Anwendung im Release-Modus ausgeführt wird, wird die Anwendung **nicht** statischen Dateien dienen. Es ist den Zuständigkeitsbereich der Webserver Verteilung die Dateien. Für diese Anwendung wird IIS statischen Dateien aus dienen `/static`.

Die Sammlung von statischen Dateien erfolgt automatisch als Teil der Bereitstellungsskript, indem zuvor Gesammelte Dateien. Dies bedeutet Collection erfolgt auf jede Bereitstellung, verlangsamt Bereitstellung etwas, aber es wird sichergestellt, dass veraltete Dateien, vermeiden ein Sicherheitsrisiko entsteht nicht zur Verfügung.

Wenn Sie eine Sammlung von statischen Dateien für eine Anwendung Django überspringen möchten:

    \.skipDjango

Klicken Sie dann müssen Sie die Sammlung manuell auf dem lokalen Computer ausführen:

    env\scripts\python manage.py collectstatic

Entfernen Sie die `\static` Ordner aus `.gitignore` und Git Repository hinzuzufügen.


## <a name="troubleshooting---settings"></a>Problembehandlung - Einstellungen

Verschiedene Einstellungen für die Anwendung geändert werden können, `DjangoWebProject/settings.py`.

Zur Vereinfachung Developer ist Debuggen-Modus aktiviert. Übersichtliche anwenden, ist, dass Sie zu Bildern und anderen statischen Inhalt angezeigt werden, wenn lokal, ohne dass statische Dateien zu sammeln ausgeführt werden.

So deaktivieren Sie Debuggen-Modus

    DEBUG = False

Wenn das Debuggen deaktiviert ist, wird der Wert für `ALLOWED_HOSTS` zum Einschließen der Azure Hostname aktualisiert werden soll. Beispiel:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

Alternativ können Sie eine zu aktivieren:

    ALLOWED_HOSTS = (
        '*',
    )

In der Praxis möchten Sie möglicherweise etwas komplexer Umgang mit Umschalten zwischen Debuggen, und lassen Sie wieder los Modus und der Hostname erste führen.

Sie können die Umgebungsvariablen über das Azure Portal **Konfigurieren** Seite im Abschnitt **Einstellungen für die app** festlegen.  Dies kann nützlich sein, zum Festlegen von Werten, die möglicherweise nicht in den Quellen (Verbindungszeichenfolgen, Kennwörter usw.) angezeigt werden soll, oder Sie zwischen Azure und Ihrem lokalen Computer anders festlegen möchten. In `settings.py`, Sie können die Umgebungsvariablen mit Abfragen `os.getenv`.


## <a name="using-a-database"></a>Verwenden einer Datenbank

Die Datenbank, die in der Anwendung enthalten ist, ist einer Sqlite Datenbank. Dies ist eine geeignete und hilfreiche Standarddatenbank für die Entwicklung verwenden, da fast keine Setup erforderlich ist. Die Datenbank wird in der Datei db.sqlite3 in den Ordner des Projekts gespeichert.

Azure bietet eine Datenbank-Services aus einer Anwendung Django problemlos verwendet werden. Lernprogramme für die Verwendung der [SQL-Datenbank] und [MySQL] aus einer Anwendung Django zeigt, welche Schritte erforderlich sind, um die Datenbank zu erstellen, ändern die Einstellung für die Datenbank im `DjangoWebProject/settings.py`, und die Bibliotheken, die für die Installation erforderlich.

Natürlich, wenn Sie Ihre eigenen Datenbankserver verwalten möchten, können Sie hierfür verwenden Windows oder Linux virtuellen Computern Azure.


## <a name="django-admin-interface"></a>Django Administrator-Benutzeroberfläche

Nachdem Sie die Erstellung Ihrer Modelle beginnen, sollten Sie die Datenbank mit einigen Daten zu füllen. Hinzufügen und Bearbeiten von Inhalten interaktiv führen Sie eine einfache Möglichkeit besteht darin, die Django Administration Benutzeroberfläche verwenden.

Der Code für die Benutzeroberfläche Administrator ist in der Anwendung Quellen kommentiert, aber es deutlich gekennzeichnet wird, damit Sie ihn (Suche nach 'Admin') einfach aktivieren können.

Nachdem sie aktiviert ist, synchronisieren Sie die Datenbank, führen Sie die Anwendung, und navigieren Sie zu `/admin`.


## <a name="next-steps"></a>Nächste Schritte

Führen Sie die folgenden Links, um weitere Informationen zur Django und Python Tools für Visual Studio:

- [Django Dokumentation]
- [Python-Tools für Visual Studio-Dokumentation]

Informationen zur Verwendung von SQL-Datenbank und MySQL:

- [Django und MySQL auf Azure Python-Tools für Visual Studio]
- [Django und SQL-Datenbank auf Azure Python-Tools für Visual Studio]

Weitere Informationen finden Sie unter der [Python Developer Center](/develop/python/).


## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Django und MySQL auf Azure Python-Tools für Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django und SQL-Datenbank auf Azure Python-Tools für Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL-Datenbank]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK für Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK für Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[Git für Windows]: http://msysgit.github.io/
[GitHub für Windows]: https://windows.github.com/
[Python-Tools für Visual Studio]: http://aka.ms/ptvs
[Python 2.2-Tools für Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python-Tools für Visual Studio-Dokumentation]: http://aka.ms/ptvsdocs
[Django Dokumentation]: https://www.djangoproject.com/
