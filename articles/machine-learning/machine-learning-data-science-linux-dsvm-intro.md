<properties
    pageTitle="Bereitstellung von Linux Daten Wissenschaft virtuellen Computers | Microsoft Azure"
    description="Konfigurieren Sie und erstellen Sie eine Linux Daten Wissenschaft virtuellen Computern auf Azure Analytics und Schulung Computer."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Bereitstellen der Linux Daten Wissenschaft virtuellen Computern

Linux Daten Wissenschaft virtuellen Computers ist eine Azure-virtuellen Computern, die Teil einer Sammlung von Tools vorinstalliert ist. Diese Tools werden häufig verwendet, um Daten Analytics und Schulung Computer. Die wichtigsten Softwarekomponenten enthalten sind:

- Microsoft R Server Developer Edition
- Anaconda Python Verteilung (Versionen 2.7 und 3.5), einschließlich beliebte Data Analysis-Bibliotheken
- JupyterHub - eine mehreren Benutzern Jupyter Notizbuch-Server R Python, aufzunehmen Kernels unterstützen
- Azure-Speicher-Explorer
- Azure line Interface (CLI) für die Verwaltung von Azure Ressourcen
- PostgresSQL-Datenbank
- Schulung von Werkzeugmaschinen
    - [Berechnete Netzwerk-Toolkit (CNTK)](https://github.com/Microsoft/CNTK): eine tief learning Toolkit Software von Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): einen schnellen Computer learning System unterstützende Techniken online, hashing, Allreduce, Reduzierung, learning2search, aktiv, und interaktive Schulung.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): ein Tool schnell und präzise stärkere Struktur Implementierung bereitstellt.
    - [Rattle](http://rattle.togaware.com/) (der R Analytical Tool zu erfahren Sie einfach): ein Tool, das erste Schritte mit Daten Analytics und Computer in R mit Durchsuchen von Benutzeroberflächen-basierten Daten ganz einfach Lern- und modellieren mit automatischen R Code generieren herstellt.
- Azure SDK in Java, Python, node.js, Ruby, PHP
- R und Python für Bibliotheken in Azure maschinellen Lern- und andere Azure Services verwenden
- Entwicklungstools und Editoren (Ellipse, Emacs, Gedit, vi)

Ausführen von Daten für Wissenschaft umfasst durchlaufen auf eine Abfolge von Aufgaben:

1. Suchen, laden und vorab verarbeiten von Daten
2. Erstellen und Testen von Datenmodellen
3. Bereitstellen von der Modelle für die Ernährung in intelligenten Clientanwendungen

Daten Wissenschaftler verwenden Sie verschiedene Tools zum Ausführen dieser Aufgaben. Sie können ganz Zeit in Anspruch nehmen zum Suchen nach den entsprechenden Versionen der Software, und klicken Sie dann zum Herunterladen kompilieren und installieren diese Versionen.

Die Linux Daten Wissenschaft virtuellen Computern kann diese Last erheblich vereinfachen. Führen sie mit Ihrem Projekt Analytics sofort. Sie können die Arbeit an Vorgängen in verschiedenen Sprachen, einschließlich R, Python, SQL, Java und C++. "Ellipse" bietet eine IDE zum Entwickeln und Testen Sie den Code, der einfach zu verwenden. Das Azure SDK enthalten, die auf dem virtuellen Computer können Sie Ihre Programme mithilfe von verschiedenen Dienste auf Linux für Microsoft Cloud-Plattform erstellen. Darüber hinaus haben Sie Zugriff auf Weitere Sprachen wie Ruby, Perl, PHP und node.js, die vorab auch installiert werden.

Es gibt keine Software Gebühren für diese Daten Wissenschaft virtueller Computer Abbildung aus. Sie Zahlen nur die Azure Hardware-Nutzungsgebühren, die bewertet werden je nach Größe des virtuellen Computers, die Sie mit dem Bild virtueller Computer bereitstellen. Weitere Informationen über die Gebühren berechnen finden Sie auf die [virtuellen Computer Angebot Seite auf Azure Marketplace ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie eine Linux Daten Wissenschaft virtuellen Computern erstellen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**: eine erhalten, finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/free/).
- **Ein Azure-Speicher-Konto**: um eine zu erstellen, finden Sie unter [Erstellen eines Kontos Azure-Speicher](storage-create-storage-account.md#create-a-storage-account). Alternativ können Sie das Speicherkonto als Teil der Vorgang des Erstellens des virtuellen Computer erstellt werden, wenn Sie kein vorhandenes Konto verwenden möchten.


## <a name="create-your-linux-data-science-virtual-machine"></a>Erstellen Sie Ihre Daten Linux Wissenschaft virtuellen Computern

Hier sind die Schritte zum Erstellen einer Instanz von Linux Daten Wissenschaft virtuellen Computern ein:

1.  Navigieren Sie zu der listing im [Portal Azure](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)-virtuellen Computern.
2.   Klicken Sie auf **Erstellen** (unten), um den Assistenten aufzurufen. ![konfigurieren-Daten-Wissenschaft-virtueller Computer](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   Die folgenden Abschnitte enthalten die Eingaben für die einzelnen Schritte des Assistenten (auf der rechten Seite der vorherigen Abbildung aufgelistet) zum Erstellen des Microsoft Data Wissenschaft virtuellen Computers verwendet. Hier sind die Eingaben zum Konfigurieren der einzelnen Schritte erforderlich sind:

    ein. **Grundlagen**:

  - **Name**: Name Ihres Daten Wissenschaft Servers, die Sie erstellen.
  - **Benutzername**: erste Konto anmelden ID an.
  - **Kennwort**: erste Kontokennwort (Sie können SSH öffentlichen Schlüssel statt Kennwort verwenden).
  - **Abonnements**: Wenn Sie mehr als ein Abonnement besitzen, wählen Sie eine Grundlage der Computer ist, erstellt und in Rechnung gestellt werden. Sie müssen die Ressource über Berechtigungen für dieses Abonnement.
  - **Ressourcengruppe**: Sie können einen neuen erstellen oder eine vorhandene Gruppe verwenden.
  - **Standort**: Wählen Sie das Data Center, die am besten geeignet ist. Normalerweise ist es das Data Center, das meisten der Daten, oder Ihren physischen Speicherort für die schnellste Netzwerkzugriff am nächsten ist.

    b. **Größe**:

  - Wählen Sie eine der Servertypen, die Ihre funktionsübergreifendes Anforderung und Kosten Einschränkungen entspricht. Wählen Sie auf **Alle anzeigen** , um weitere Auswahlmöglichkeiten virtueller Computer Größen angezeigt werden.

    c. **Einstellungen**:

  - **Datenträger Type**: Wählen Sie **Premium** aus, wenn Sie eine einfarbige State-Laufwerk (SSD lieber). Wählen Sie andernfalls **Standard**.
  - **Speicher-Konto**: erstellen ein neuen Kontos mit Azure-Speicher im Rahmen Ihres Abonnements oder verwenden eine vorhandenen an derselben Stelle, die ausgewählt wurde, die **Grundlagen** Schritt des Assistenten.
  - **Andere Parameter**: In den meisten Fällen verwenden Sie nur die Standardwerte. Zu berücksichtigen sind, nicht standardmäßigen Werten, zeigen Sie auf den Link Hilfe zu den spezifischen Feldern informativen.

    d. **Zusammenfassung**:

  - Stellen Sie sicher, dass alle eingegebenen Informationen richtig ist.

    e. **Kaufen**:

  - Klicken Sie auf **kaufen**, um die Bereitstellung zu starten. Ein Link wird die Vertragsbedingungen der Transaktion bereitgestellt. Der virtuellen Computer verfügt keine zusätzlichen Gebühren jenseits der Computer für die Servergröße, die Sie in der **Größe** Schritt ausgewählt haben.

Die Bereitstellung sollte ungefähr 10 bis 20 Minuten. Der Status der Bereitstellung wird der Azure-Portal angezeigt.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Zugreifen auf die Linux Daten Wissenschaft virtuellen Computern

Nachdem Sie der virtuellen Computer erstellt wurde, können Sie darauf melden Sie sich mithilfe von SSH. Verwenden Sie die Anmeldeinformationen, die Sie im Abschnitt **Grundlagen** von Schritt 3 für die Schnittstelle der Text-Verwaltungsshell erstellt. Klicken Sie auf Windows können Sie ein SSH-Client-Tool wie [kitten](http://www.putty.org)herunterladen. Falls gewünscht einen grafischen Desktop (X Windows-System), können Sie auf kitten weiterleiten X11 verwenden oder installieren Sie den X2Go Client.

>[AZURE.NOTE] Der X2Go-Client durchgeführt erheblich besser als X11 testen weiterleiten. Es empfiehlt sich, mit dem X2Go-Client für eine grafisch desktop-Oberfläche.


## <a name="installing-and-configuring-x2go-client"></a>Installieren und Konfigurieren von X2Go-client

Die Linux VM ist bereits mit X2Go Server bereitgestellten und bereit sind, Clientverbindungen zu übernehmen. Zum Verbinden mit den Linux VM grafisch Desktop, gehen Sie auf Ihren Kunden:

1. Herunterladen Sie und installieren Sie den X2Go-Client für Ihre Clientplattform aus [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Führen Sie die X2Go, und wählen Sie **Neue Sitzung**. Es wird ein Konfigurationsfenster mit mehreren Registerkarten geöffnet. Geben Sie die folgenden Konfigurationsparameter aus:
    * **Registerkarte Sitzung**:
        - **Host**: der Hostname oder die IP-Adresse des Ihrer Linux Daten Wissenschaft virtueller Computer.
        - **Benutzername**: Benutzername auf die Linux VM.
        - **SSH Port**: lassen sie das am 22, den Standardwert.
        - **Sitzungstyp**: Ändern Sie den Wert in XFCE. Die Linux VM unterstützt derzeit nur XFCE Desktop.
    * **Registerkarte Medien**: Sie können sound Support und drucken, wenn Sie benötigen, deren Verwendung Client deaktivieren.
    * **Freigegebene Ordner**: Wenn Sie von Ihren Clientcomputern bereitgestellt werden, klicken Sie auf die Linux VM Verzeichnisse durchsuchen möchten, hinzufügen die Client Computer Verzeichnisse durchsuchen, die Sie für den virtuellen Computer auf dieser Registerkarte freigeben möchten.

Nachdem Sie auf den virtuellen Computer melden Sie sich mithilfe der SSH-Client oder XFCE grafisch Desktop über den Client X2Go, sind Sie bereit sind, verwenden die Tools, die installiert und des virtuellen Computers konfiguriert sind. Klicken Sie auf XFCE können Sie Tastenkombinationen für Applikationen Menüs und desktop-Symbole für viele der Tools anzeigen.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Tools auf Linux Daten Wissenschaft virtuellen Computern installiert

### <a name="microsoft-r-open"></a>Microsoft R öffnen
R ist eine der am häufigsten verwendeten Sprachen für die Datenanalyse und Computer-Schulung. Wenn Sie für Ihre Analytics R verwenden möchten, hat der virtuellen Computer Microsoft R öffnen (MRO) mit mathematischen Kernel Library (MKL). Die MKL optimiert mathematische Operationen in analytical Algorithmen häufig. MRO zu 100 Prozent mit CRAN-R kompatibel ist, und eine der im CRAN veröffentlicht R Bibliotheken kann auf die MRO installiert werden. Sie können Ihre R-Programme in einem der Standardeditoren, wie vi, Emacs oder Gedit bearbeiten. Sie können auch herunterladen und andere IDEs, z. B. [RStudio](http://www.rstudio.com)verwenden. Erleichtern wird ein einfaches Skript (installRStudio.sh) im Verzeichnis **/dsvm/tools** bereitgestellt, die RStudio installiert. Wenn Sie den Emacs-Editor verwenden, wurde Beachten Sie, dass die Emacs ESS (Emacs spricht Statistiken), Verpacken, um zu vereinfachen arbeiten mit R Dateien innerhalb des Editors Emacs vorinstalliert.

Um R starten möchten, geben Sie einfach **R** in der Shell. Dadurch gelangen Sie zu einer interaktiven Umgebung. Bei der Entwicklung Ihrer R Programm verwenden Sie einen Editor wie Emacs oder vi oder Gedit in der Regel, und führen Sie dann die Skripts innerhalb R. Wenn Sie RStudio installiert haben, müssen Sie eine vollständige grafische IDE-Umgebung, die Ihr Programm R entwickeln möchte.

Es gibt auch ein R-Skript für die [oberen 20 R Pakete](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) zu installieren, wenn Sie möchten. Dieses Skript kann ausgeführt werden, nachdem Sie die interaktive Benutzeroberfläche R, die sind (wie erwähnt) eingegeben werden können, indem Sie Sie in die Verwaltungsshell **R** eingeben.  

### <a name="python"></a>Python
Für die Entwicklung mithilfe von Python wurde Anaconda Python Verteilung 2.7 und 3.5 installiert. Diese Verteilung enthält die Basis Python zusammen mit etwa 300 der am häufigsten verwendeten Mathematik, technisch und Daten Analytics-Pakete. Sie können die Standard-Text-Editoren verwenden. Darüber hinaus können Sie Spyder, eine Python IDE an, die im Paket mit Anaconda Python Verteilung befindet. Spyder benötigt einen grafisch Desktop oder X11 weiterleiten. Eine Verknüpfung zu Spyder werden in den grafischen Desktop bereitgestellt.

Da wir sowohl Python 2.7 und 3.5 haben, müssen Sie die gewünschte Version der Python speziell zu aktivieren, die Sie in die aktuelle Sitzung arbeiten möchten. Der Aktivierungsprozess legt die PATH-Variable auf die gewünschte Version Python.

Wenn Python 2.7 aktivieren möchten, führen Sie Folgendes über die Verwaltungsshell aus:

    source /anaconda/bin/activate root

Python 2.7 wird unter */anaconda/bin*installiert.

Wenn Python 3.5 aktivieren möchten, führen Sie Folgendes über die Verwaltungsshell aus:

    source /anaconda/bin/activate py35


Python 3.5 wird unter */anaconda/envs/py35/bin*installiert.

Um eine interaktive Python-Sitzung aufzurufen, geben Sie **Python** einfach die Verwaltungsshell. Wenn Sie auf eine Benutzeroberfläche oder Festlegen von Weiterleitung X11 haben, können Sie **Spyder** zum Starten der IDE Python eingeben.

### <a name="jupyter-notebook"></a>Jupyter Notizbuch

Die Verteilung Anaconda Lieferumfang auch ein Jupyter Notizbuch, einer Umgebung Code und Analyse freigeben. Der Zugriff auf das Notizbuch Jupyter erfolgt über JupyterHub. Sie melden Sie sich mit Ihrem lokalen Linux-Benutzernamen und Ihr Kennwort ein.

Der Server Jupyter Notizbuch wurde mit Python 2, 3 Python und R Kernels vorab konfiguriert. Es wird ein Desktopsymbol mit dem Namen "Jupyter Notizbuch" Starten Sie den Browser, um den Server Notizbuch zugreifen. Wenn Sie des virtuellen Computers über SSH oder X2Go Client sind, können Sie auch besuchen [Https://localhost:8000 /](https://localhost:8000/) auf den Jupyter Notizbuch Server zugreifen.

>[AZURE.NOTE] Fahren Sie, wenn Sie alle Warnungen Zertifikat erhalten.

Sie können den Jupyter Notizbuch Server von jedem Host zugreifen. Geben Sie einfach *https://\<VM DNS-Namen oder die IP-Adresse\>: 8000 /*

>[AZURE.NOTE] Port 8000 wird in der Firewall standardmäßig geöffnet, wenn Sie der virtuellen Computer bereitgestellt wird.

Wir haben Stichprobe Notizbücher – eine in Python und eine im r verpackt. Sie können den Link zu den Beispielen auf der Startseite Notizbuch anzeigen, nachdem Sie in das Notizbuch Jupyter authentifizieren, indem Sie mit Ihrem lokalen Linux-Benutzernamen und Kennwort. Sie können ein neues Notizbuch, indem Sie **neu**, und klicken Sie dann den geeigneten Sprache Kernel erstellen. Wenn Sie die Schaltfläche " **neu** " angezeigt werden, klicken Sie auf das Symbol **Jupyter** oben links, um zur Homepage des Servers Notizbuch zu wechseln.


### <a name="ides-and-editors"></a>IDEs und Editoren

Sie haben die Wahl der mehrere Code-Editoren. Dies umfasst vi/VIM, Emacs, gEdit und "Ellipse" ein. gEdit "Ellipse" sind grafisch Editoren und möchte Sie mit einem grafischen Desktop angemeldet sein, um diese zu verwenden. Diesen Editoren haben Desktop- und Anwendung Menü Tastenkombinationen, um diese zu starten.

**VIM** und **Emacs** sind textbasierten Editoren. Klicken Sie auf Emacs haben wir ein Add-On-Paket aufgerufen Emacs spricht Statistiken (ESS), die Arbeit mit R im Emacs-Editor vereinfacht installiert. Weitere Informationen finden Sie unter [ESS](http://ess.r-project.org/)werden.

**"Ellipse"** ist eine offene Quelle, erweiterbaren IDE, die mehrere Sprachen unterstützt. Java-Entwickler-Edition ist die Instanz des virtuellen Computers installiert. -Plug-Ins sind verfügbar für mehrere beliebte Sprachen, die installiert werden können, um die Ellipse Umgebung zu erweitern. Wir haben auch ein Plug-In installiert in "Ellipse" **Azure-Toolkit für "Ellipse"**bezeichnet. Sie können Sie erstellen, entwickeln, testen und Bereitstellen von Azure Applications mithilfe der Ellipse Entwicklung-Umgebung, die die Sprachen wie Java unterstützt. Es gibt auch ein **Azure SDK für Java** , die Zugriff auf die verschiedenen Azure Diensten in einer Java-Umgebung aus ermöglicht. Weitere Informationen zum Azure-Toolkit für "Ellipse" kann bei [Azure-Toolkit für "Ellipse"](../azure-toolkit-for-eclipse.md)gefunden werden.

**LaTex** wird über das Paket Texlive sowie ein Emacs Add-On- [Auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) -Paket installiert, um Ihre Dokumente LaTex innerhalb Emacs authoring zu vereinfachen.  

### <a name="databases"></a>Datenbanken

#### <a name="postgres"></a>Postgres
Die open Source-Datenbank **Postgres** steht des virtuellen Computers mit den Diensten, die ausgeführt und Initdb bereits abgeschlossen wurde. Sie müssen immer noch Datenbanken und Benutzer zu erstellen. Weitere Informationen finden Sie in der [Dokumentation Postgres](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Grafische SQL client
**Eichhörnchen SQL**, ein grafisch SQL-Client, wurde die Verbindung zu anderen Datenbanken (wie etwa Microsoft SQL Server, Postgres und MySQL) und zum Ausführen von SQL-Abfragen bereitgestellt. Sie können dies aus einer grafisch desktop-Sitzung (z. B. über den Client X2Go) ausführen. Um Eichhörnchen SQL aufzurufen, können Sie über das Symbol auf dem Desktop zu starten oder den folgenden Befehl aus der Shell ausgeführt.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Vor der ersten Verwendung, richten Sie Ihre Treiber und Datenbank Aliases. Die JDBC-Treiber befinden sich unter:

*/usr/share/Java/jdbcdrivers*

Weitere Informationen finden Sie unter [Eichhörnchen SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Befehlszeilenoptionen für den Zugriff auf Microsoft SQL Server-tools

Das ODBC-Treiber-Paket für SQL Server enthält auch zwei Befehlszeile Tools:

**Bcp**: die Bcp Programm Massen kopiert Daten zwischen einer Instanz von Microsoft SQL Server und einer Datendatei in einem vom Benutzer angegebenen Format. Das Programm Bcp kann große Mengen neuer Zeilen in SQL Server-Tabellen importieren oder Exportieren von Daten aus Tabellen in Datendateien verwendet werden. Sie müssen zum Importieren von Daten in einer Tabelle mithilfe einer Formatdatei für die Tabelle erstellt oder Verständnis der Struktur der Tabelle und die Arten von Daten, die für ihre Spalten gültig sind.

Weitere Informationen finden Sie unter [Verbinden mit Bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**SQLCMD**: Geben Sie mit der Sqlcmd-Programm als auch System Verfahren Transact-SQL-Anweisungen, und Skripts Dateien in die Befehlszeile eingeben. Dieses Programm verwendet ODBC zum Transact-SQL-Stapel ausführen.

Weitere Informationen finden Sie unter [Verbinden mit Sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Es gibt einige Unterschiede in dieses Programm Linux und Windows-Plattformen. Finden Sie in der Dokumentation Details.


#### <a name="database-access-libraries"></a>Datenbank Access-Bibliotheken

Es gibt Bibliotheken in R und Python für Access-Datenbanken verfügbar.

- In R ermöglicht die **RODBC** oder **Dplyr** -Paket Abfragen oder SQL-Anweisungen auf dem Datenbankserver ausführen.
- In Python bietet die Bibliothek **Pyodbc** Datenbankzugriff mit ODBC als die darunter liegende Ebene.  

Auf **Postgres**zugreifen zu können:

- Verwenden Sie das Paket **RPostgreSQL**R:.
- Python: Verwenden Sie die Bibliothek **psycopg2** .


### <a name="azure-tools"></a>Azure-tools
Die folgenden Azure Tools werden des virtuellen Computers installiert:

- **Azure Line-Benutzeroberfläche**: das Azure CLI ermöglicht es Ihnen, erstellen und Verwalten von Azure Ressourcen über Shell-Befehle. Um die Azure Tools aufzurufen, geben Sie einfach **Azure-Hilfe**. Weitere Informationen finden Sie unter der [Azure CLI Dokumentationsseite](../virtual-machines-command-line-tools.md).
- **Microsoft Azure-Speicher-Explorer**: Microsoft Azure-Speicher-Explorer ist ein grafischen Tool, die durch die Objekte zu navigieren, die in Ihrem Konto Azure Storage gespeichert sind, und zum Hochladen und Herunterladen von Daten an und von Azure Blobs verwendet wird. Sie können das Symbol für die Verknüpfung auf dem desktop Speicher-Explorer zugreifen. Sie können es aus einer Shell-Aufforderung mit der Eingabe **StorageExplorer**aufrufen. Sie müssen ein X2Go-Desktopclient angemeldet sein, oder Weiterleitung Festlegen von X11 haben.
- **Azure Bibliotheken**: im folgenden sind einige der integrierten Bibliotheken.

 - **Python**: der Azure-bezogene Bibliotheken in Python, die installiert werden, sind **Azure**, **Azureml**, **Pydocumentdb**und **Pyodbc**. Mit den ersten drei Bibliotheken können Sie Azure-Speicherservices, Azure maschinellen Lern- und Azure DocumentDB (eine NoSQL-Datenbank auf Azure) zugreifen. Der vierte Bibliothek Pyodbc (zusammen mit der Microsoft-ODBC-Treiber für SQL Server), ermöglicht Zugriff auf SQL Server, SQL Azure-Datenbank und Azure SQL-Data Warehouse aus Python durch eine ODBC-Schnittstelle an. Geben Sie **Pip-Liste** , um alle aufgelisteten Bibliotheken anzuzeigen. Achten Sie darauf, dass Sie sowohl die Python 2.7 und 3,5 Umgebungen diesen Befehl ausführen.
 - **R**: der Azure-bezogene Bibliotheken in R, die installiert werden sind **AzureML** und **RODBC**.
 - **Java**: die Liste der Bibliotheken Azure Java finden Sie in dem Verzeichnis **/dsvm/sdk/AzureSDKJava** des virtuellen Computers. Die wichtigsten Bibliotheken sind Azure Speicherung und Management-APIs, DocumentDB und JDBC-Treiber für SQL Server.  

Sie können die integrierten Firefox-Browser der [Azure-Portal](https://portal.azure.com) zugreifen. Klicken Sie im Portal Azure können Sie erstellen, verwalten und Azure Ressourcen überwachen.

### <a name="azure-machine-learning"></a>Learning Azure-Computern

Azure maschinellen Learning ist eine vollständig verwaltete Cloud-Dienst, mit dem Sie zum Erstellen und Bereitstellen Vorhersageanalytik Lösungen freigeben. Erstellen Sie Ihre Versuche und Modelle aus Azure maschinellen Learning Studio an. Sie können mit einem Webbrowser auf die Daten Wissenschaft virtuellen Computern finden Sie auf [Microsoft Azure maschinellen Learning](https://studio.azureml.net)zugegriffen werden.

Nach der Anmeldung Azure maschinellen Learning Studio haben Sie Zugriff auf eine experimentieren Zeichenbereich, in dem Sie einen logischen Fluss für den Computer learning Algorithmen erstellen. Sie auch Zugang zu einem Jupyter Notizbuch auf Azure maschinellen Learning gehostet und nahtlos mit die Versuche in Computer Learning Studio arbeiten können. Prozessen umsetzen des Computers learning Modelle, die Sie erstellt haben, indem Sie sie in eine Webdienstschnittstelle einbinden. Dadurch können Clients, die in einer beliebigen Sprache geschrieben Vorhersagen vom Computer learning Modelle aufrufen. Weitere Informationen finden Sie unter der [Computer Learning-Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

Sie können auch Ihre Modelle in R oder Python des virtuellen Computers erstellen, und klicken Sie dann in Betrieb auf Azure maschinellen Learning bereitstellen. Wir haben Bibliotheken in R (**AzureML**) und Python (**Azureml**), aktivieren Sie dieses Feature installiert.

Informationen zum Bereitstellen von Datenmodellen in R und Python in Azure maschinellen Learning finden Sie unter [zehn Punkte, die Sie über die Daten Wissenschaft virtuellen Computern ausführen können](machine-learning-data-science-vm-do-ten-things.md) (insbesondere im Abschnitt "Modelle mit R oder Python erstellen und Prozessen umsetzen können mit Azure maschinellen Learning").

>[AZURE.NOTE] Diese Anweisungen wurden für die Windows-Version von den Daten Wissenschaft virtuellen Computer geschrieben werden. Aber die Angaben zum Bereitstellen von Datenmodellen Azure Computer lernen besteht die Linux VM anwendbar.

### <a name="machine-learning-tools"></a>Schulung von Werkzeugmaschinen

Im Lieferumfang von des virtuellen Computer sind ein paar Computer learning Tools und Algorithmen, die vorab kompiliert und vorab lokal installiert wurden. Hierzu gehören:

* **CNTK** (Berechnete Netzwerk Toolkit von Microsoft Research): eine tief learning Toolkit.
* **Vowpal Wabbit**: einen schnellen online Learning-Algorithmus.
* **Xgboost**: ein Tool, optimierte, stärkere Struktur Algorithmen bereitstellt.
* **Python**: Anaconda Python gebündelten mit maschinellen Learning Algorithmen mit Bibliotheken wie Scikit Informationen enthält. Sie können andere Bibliotheken installieren, indem Sie mit der `pip install` Befehl.
* **R**: eine umfangreiche Bibliothek von maschinellen Learning Funktionen steht für R. Einige der Bibliotheken, die bereits installiert sind, sind lm, Glm, RandomForest, Rpart. Andere Bibliotheken können installiert werden, indem Sie ausführen:

        install.packages(<lib name>)

Hier sind einige zusätzliche Informationen über die ersten drei Computer Learning-Tools in der Liste ein.

#### <a name="cntk"></a>CNTK
Dies ist eine offene Quelle, Tiefe learning Toolkit. Es ist ein Befehlszeile Tool (Cntk) und befindet sich bereits in den Pfad.

Wenn Sie ein einfaches Beispiel ausführen zu können, führen Sie die folgenden Befehle in der Shell aus:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Die Ausgabe Modell befindet sich *~/cntkdemo/Output/Models*.

Weitere Informationen finden Sie unter Abschnitt CNTK [GitHub](https://github.com/Microsoft/CNTK)und das [CNTK Wiki](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit ist ein Computer, der learning System, Techniken online, hashing, Allreduce, Reduzierung, learning2search, aktiv, verwendet, und interaktive Schulung.

Wenn Sie das Tool auf ein einfaches Beispiel dafür ausführen zu können, führen Sie folgende Schritte aus:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Es gibt andere, größere Demos in diesem Verzeichnis ein. Weitere Informationen über die Angabe finden Sie unter [Dieser Abschnitt GitHub](https://github.com/JohnLangford/vowpal_wabbit)und das [Vowpal Wabbit Wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Dies ist eine Bibliothek, die entworfen und optimiert für Algorithmen stärkere (Struktur). Ziel dieser Bibliothek ist die Berechnung Grenzwerte Maschinen an den äußersten zum Bereitstellen von umfangreichen verstärken, die Struktur benötigt übertragen skalierbare, tragbaren und genau ist.

Es wird als eine Befehlszeile ebenso wie eine Bibliothek R bereitgestellt.

Wenn diese Bibliothek in R verwenden möchten, können Sie zunächst eine interaktive R Sitzung (nur **R** die Verwaltungsshell), und laden die Bibliothek.

Hier ist ein einfaches Beispiel, das Sie in der Aufforderung R ausführen können:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Führen Sie die Befehlszeile Xgboost sind hier die Befehle in der Shell ausführen:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Eine .model-Datei wird in dem angegebenen Verzeichnis geschrieben. Informationen zu diesem Beispiel Demo finden Sie [auf GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Weitere Informationen zu Xgboost finden Sie unter der [Xgboost Dokumentationsseite](https://xgboost.readthedocs.org/en/latest/)und deren [Github Repository](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle ( **R** **A**Nalytical **T**Ool **T**o **L**verdienen **E**Asily) werden Durchsuchen von Daten Benutzeroberflächen-basierten und Modellierung verwendet. Statistische oder visuelle Zusammenfassung der Daten, Sie können Daten, die problemlos verwendet werden können, unbeaufsichtigt und Kontrolle Modelle aus den Daten erstellt, die Leistung von Datenmodellen grafisch, bietet Sicherheitsstandards und legt die Bewertungen der Lesbarkeit neue Daten. Er generiert außerdem R Code, repliziert die Vorgänge in der Benutzeroberfläche, die direkt in R ausführen oder als Ausgangspunkt für eine weitere Analyse verwendet werden können.

Um Rattle ausführen zu können, müssen Sie in einer grafischen desktop-in-Sitzung werden. Geben Sie auf dem Terminal ```R``` die Umgebung R eingeben. R aufgefordert werden Geben Sie die folgenden Befehle:

    library(rattle)
    rattle()

Nun wird eine Benutzeroberfläche von mit einer Reihe von Registerkarten geöffnet. Hier sind die Schritte Schnellstart in Rattle zum Verwenden einer Stichprobe Wetter Datenmenge und Erstellen eines Modells erforderlich sind. In einigen der Schritte werden Sie aufgefordert, automatisch installieren und Laden einige erforderliche R Pakete, die noch nicht auf dem System sind.

>[AZURE.NOTE] Wenn Sie Zugriff auf das Paket im Systemverzeichnis (die Standardeinstellung) installiert haben, wird möglicherweise eine Aufforderung in Ihrem R Console-Fenster auf Pakete auf Ihre persönliche Bibliothek installieren angezeigt. Beantworten Sie *y* , wenn Sie diese Anweisungen finden Sie unter.

1. Klicken Sie auf **Ausführen**.
2. Ein Dialogfeld eingeblendet, die fragt, ob Sie Beispiel Wetter Datenmenge verwenden möchten. Klicken Sie auf **Ja,** um das Beispiel zu laden.
3. Klicken Sie auf der Registerkarte **Modell** .
4. Klicken Sie auf **Ausführen** , erstellen Sie eine Entscheidungsstruktur an.
5. Klicken Sie auf **Zeichnen** , um die Entscheidungsstruktur anzuzeigen.
6. Klicken Sie auf das Optionsfeld **Gesamtstruktur** , und klicken Sie auf **Ausführen** , um eine zufällige Gesamtstruktur erstellen.
7. Klicken Sie auf der Registerkarte **Auswerten** .
8. Klicken Sie auf das Optionsfeld **Risiko** , und klicken Sie auf **Ausführen** , um zwei Risiko (kumuliert) Leistung Flächen anzuzeigen.
9. Klicken Sie auf die Registerkarte **Protokoll** , um den Code generieren R für die vorherige Vorgänge anzuzeigen.
(Auf einen Fehler in der aktuellen Version von Rattle, müssen Sie Einfügen einer *#* vor *Exportieren dieses Protokoll...* im Text der Log-Zeichen.)
10. Klicken Sie auf die Schaltfläche **Exportieren** , um die R Skriptdatei mit dem Namen *Weather_script speichern. R* in den Ordner "Privat".

Beenden von Rattle und R. Sie können jetzt das generierte R Skript ändern, oder verwenden, wie es ist, jederzeit ausführen, um alles zu wiederholen, die innerhalb der Benutzeroberfläche für Rattle ausgeführt wurde. Dies ist insbesondere für Anfänger in R eine einfache Möglichkeit, schnell Analyse ausführen und Computer Learning in eine übersichtliche Benutzeroberfläche, bei der Erstellung automatisch Code in R zu ändern und/oder erfahren.


## <a name="next-steps"></a>Nächste Schritte
Hier ist, wie Sie Ihre Lern- und datenauswertung weiterhin können:

* Die [Daten Wissenschaft auf Linux Daten Wissenschaft virtuellen Computern](machine-learning-data-science-linux-dsvm-walkthrough.md) Exemplarische Vorgehensweise wird gezeigt, wie mehrere allgemeine Daten Wissenschaft Aufgaben mit den virtuellen ' Linux Daten Wissenschaft Computer nach der Bereitstellung hier. 
* Erforschen von den verschiedenen Wissenschaft Datentools der Daten Wissenschaft virtueller Computer, indem Sie die Tools in diesem Artikel beschriebenen ausprobieren. Sie können auch *Dsvm-Weitere-Info* auf die Verwaltungsshell des virtuellen Computers für eine Einführung in die Grundlagen und Zeiger zu weiteren Informationen über die Tools des virtuellen Computers installiert ausführen.  
* Erfahren Sie, wie analytical End-to-End-Lösungen mithilfe des [Team Daten Wissenschaft Prozess](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)systematisch zu erstellen.
* Finden Sie auf der [Cortana Analytics Katalog](http://gallery.cortanaanalytics.com) für maschinelle Lern- und Daten Analytics Beispiele, in denen die Cortana Analytics-Suite verwenden.
