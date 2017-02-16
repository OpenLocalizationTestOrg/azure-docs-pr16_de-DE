<properties
    pageTitle="Eigenschaften der Azure-Rolle"
    description="Erfahren Sie, wie Sie mit dem Azure-Toolkit für "Ellipse" Azure-Rolle Einstellungen konfigurieren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Eigenschaften der Azure-Rolle #

Verschiedene Konfigurationen für Ihre Rolle Azure können innerhalb der Azure-Toolkit für "Ellipse" festgelegt werden.

## <a name="configuring-azure-role-properties"></a>Konfigurieren der Eigenschaften für Azure-Rolle ##

Konfigurieren der Eigenschaften für Ihre Azure-Rolle erfolgt über die Eigenschaft Dialogfelder für Ihre Arbeitskollegen Rolle. Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, und wählen Sie im Untermenü **Azure** . (Wenn Sie die Rolle im Projekt-Explorer "Ellipse" angezeigt werden, erweitern Sie im Projekt-Explorer Projekt Azure.)

![][ic789599]

In diesem Thema werden die verschiedenen Eigenschaften, die in den Dialogfeldern **Eigenschaften** festgelegt werden können beschrieben. Beachten Sie, dass viele Eigenschaften automatisch beim Erstellen eines neuen Projekts von Azure-Bereitstellung ausgefüllt werden.

Die folgenden Eigenschaftenseiten stehen für Azure Rollen.

* [Eigenschaften von virtuellen Computern](#virtual_machine_properties)
* [Zwischenspeichern von Eigenschaften](#caching_properties)
* [Eigenschaften der Zertifikate](#certificates_properties)
* [Komponenten Eigenschaften](#components_properties)
* [Für das Debuggen Eigenschaften](#debugging_properties)
* [Endpunkte Eigenschaften](#endpoints_properties)
* [Eigenschaften der Umgebung Variablen](#environment_variables_properties)
* [Lastenausgleich / Sitzung Zugehörigkeit (bzw. "Kurznotizen Sitzungen") Eigenschaften](#session_affinity_properties)
* [Speichereigenschaften der lokalen](#local_storage_properties)
* [Eigenschaften der Server-Konfiguration](#server_configuration_properties)
* [SSL-Verschiebung Eigenschaften](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>Eigenschaften von virtuellen Computern ###

Öffnen Sie das Kontextmenü für die Rolle aus, klicken Sie im Projekt-Explorer die Ellipse **Azure**klicken Sie auf, und klicken Sie dann auf **Eigenschaften**, und Sie haben die Möglichkeit, die Größe des virtuellen Computers zu ändern, und ändern Sie die Anzahl der Instanzen, auch, wie in der folgenden Abbildung gezeigt.

![][ic719499]

>[AZURE.NOTE] Nur Windows: Wenn Sie die Anzahl der Instanzen auf einen Wert größer als 1 festgelegt, und Sie auch einen Anwendungsserver konfigurieren, wird das Toolkit zulassen nur 1 Instanz der Rolle im Emulator, unabhängig von dieser Einstellung ausführen. Dies ist Port Bindungskonflikte zwischen den verschiedenen Serverinstanzen (beispielsweise alle versuchen, die an den Port 8080 Binden) zu vermeiden, wenn sie auf dem gleichen Computer ausgeführt werden. Der Anzahl der gewünschten Instanzen festlegen wird zwar beibehalten, aber es wirksam nur, wenn Sie in der Cloud bereitstellen.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>Zwischenspeichern von Eigenschaften ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, klicken Sie auf **Azure**, und klicken Sie dann auf **Zwischenspeichern**. In diesem Dialogfeld können Sie benannte am selben Standort den Memcache kompatiblen Caches aktivieren ermöglicht es Ihnen helfen, Ihre Webanwendungen beschleunigen.

![][ic719483]

Innerhalb der Eigenschaftenseite **Zwischenspeichern** können Sie globale Einstellungen für Folgendes angeben:

* Gibt an, ob am selben Standort Zwischenspeichern aktiviert ist.
* die Cachegröße als Prozentwert des Arbeitsspeichers.
* der Name des Speicher für den Cachestatus speichern, wenn die Anwendung als Clouddienst oder keine ausgeführt wird, wenn Sie nicht im Cache Zustand speichern möchten. (Der Kontonamen Speicher wird nicht verwendet, wenn beim Ausführen der Anwendung in der Serveremulator.) Wenn Sie den Namen des Kontos Speicher auf **(Auto)** festlegen (der Standard), wird die Zwischenspeichern Konfiguration automatisch das gleiche Speicherkonto als verwendet, die Sie im Dialogfeld **Veröffentlichen in Azure** auswählen.

>[AZURE.NOTE] Die Einstellung **(automatische)** müssen auf den gewünschten Effekt nur, wenn Sie eine Bereitstellung mit dem Ellipse-Toolkit Veröffentlichen Assistenten veröffentlichen. Wenn Sie stattdessen veröffentlichen Sie die .cspkg Datei manuell mithilfe einer externen Schreibschutz, z. B. im [Verwaltungsportal Azure][]funktioniert die Bereitstellung nicht ordnungsgemäß.

Im folgende Dialogfeld zeigt die Eigenschaften für einen Cache.

![][ic719501]

* **Name:** Der Name des Caches gemeinsame befindet.
* **Port-Nummer:** Die Anzahl der Port für den Cache verwendet werden soll.
* **Ablaufrichtlinie:** Die folgenden Werte, die angibt, wenn ein Schlüssel im Cache läuft ab.
    * **Absolut:** Die Taste läuft ab, wenn die von **Minuten live** angegebene Zeit erreicht ist.
    * **NeverExpires:** Die Taste kein Ablaufdatum.
    * **SlidingWindow:** Die Taste läuft ab, wenn es für die Anzahl von **Minuten live**angegebene Zeit nicht zugegriffen wurde; jedes Mal, wenn es zugegriffen werden kann, wird die Ablauf Uhr zurückgesetzt.
* **Minuten live:** Maximale Anzahl von Minuten für eine Memcached-Taste, um live unterliegen die Ablaufrichtlinie.
* **Hohe Verfügbarkeit mit repliziert Sicherungskopien auf andere Rolleninstanzen:** Wenn aktiviert, bieten hilft Sicherungskopien auf andere Rolleninstanzen Nutzung der hohen Verfügbarkeit repliziert werden. Beachten Sie, dass mindestens zwei Rolleninstanzen facto für die Bereitstellung für dieses Feature entwickelt werden müssen.

Um einen neuen Cache hinzuzufügen, klicken Sie auf die Schaltfläche **Hinzufügen** , in die Eigenschaftenseite **Zwischenspeichern** und ein Dialogfeld **Mit dem Namen Cache konfigurieren** wird geöffnet. Geben Sie Werte für die Eigenschaften für die oben beschrieben werden.

Um einen benannten Cache zu ändern, wählen Sie aus dem Cache, und klicken Sie auf die Schaltfläche **Bearbeiten** in die Eigenschaftenseite **Zwischenspeichern** . Ein Dialogfeld wird ermöglicht es Ihnen, ändern die Eigenschaften von Cache geöffnet. Drücken Sie **OK** , um den Cache Werte zu speichern.

Wählen Sie aus dem Cache um einen Cache zu löschen, klicken Sie auf die Schaltfläche " **Entfernen** " in der Eigenschaftenseite **Zwischenspeichern** , und klicken Sie dann auf **Ja,** um den Löschvorgang zu bestätigen.

Weitere Informationen zum Zwischenspeichern von finden Sie unter [So verwenden Sie Taste Zwischenspeichern][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Eigenschaften der Zertifikate ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, klicken Sie auf **Azure**, und klicken Sie dann auf **Zertifikate**.

![][ic710964]

In diesem Dialogfeld können Sie hinzufügen oder Entfernen von Zertifikate auf Ihrem Projekt "Ellipse" verweist. Beachten Sie, dass die hier aufgeführten Zertifikate nicht automatisch in alle Java Keystore gespeichert werden und daher nicht automatisch für die Verwendung von innerhalb einer Java-Anwendung verfügbar sind. Sie sind nur mit Azure registriert, damit diese vorinstalliert werden können in der Windows-Zertifikat speichern auf deren die Bereitstellung der virtuellen Computern und anschließend verwendet durch eine andere Windows-Software. Derzeit ist das einzige Feature des Toolkits, die die Zertifikate auf diese Weise im Dialogfeld **Zertifikate** verwiesen wird verwendet, [Verschiebung SSL][]-wegen der Verwendung von Internet Information Services (IIS) und Anwendung anfordern Routing (ARR), die das richtige Zertifikat, um auf diese Weise zur Verfügung gestellt werden muss.

Wenn Sie Ihr Projekt Azure mithilfe des Veröffentlichen-Assistenten bereitstellen, werden Sie aufgefordert, zeigen Sie auf persönliche Informationen Exchange (PFX) die entsprechenden Dateien für diese Zertifikate, zusammen mit ihrer Kennwörter, damit sie automatisch in den Azure Service, aber nur, wenn sie es zuvor wurden nicht hochgeladen hochladen.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Komponenten Eigenschaften ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, klicken Sie auf **Azure**, und klicken Sie dann auf **Komponenten**. In diesem Dialogfeld haben Sie die Möglichkeit zum Hinzufügen, ändern oder Entfernen der Komponenten von Ihrer Rolle sowie Ändern der Reihenfolge, in der sie verarbeitet werden.

![][ic719502]

Das Komponenten-Feature ermöglicht es Ihnen hinzufügen Abhängigkeiten zu Ihrem Projekt Azure-Bereitstellung, wie etwa Java-Anwendungsprojekte, spezielle Dateien oder ausführbare Befehlszeile Anweisungen, die durch die Bereitstellung erforderlich sind.

Für jede Komponente können Sie Folgendes angeben:

* Der Schritt, die durchgeführt werden, wenn die Komponente in Ihrem Projekt Azure-Bereitstellung importieren, wenn dieses erstellt wird.
* Der Schritt absolviert werden, wenn Sie die Komponente in der Cloud Azure bereitstellen.

>[AZURE.NOTE] Beachten Sie bei Komponentendateien oder Zeilen zurück, der angibt, beachten Sie, dass die Bereitstellung auf einem Windows-Computer, veröffentlicht wird, damit Ihre benutzerdefinierten Schritte für ein Betriebssystem Windows-basierten gültigen werden müssen. 

Komponenten stehen die folgenden Eigenschaften:

* **Importieren:** Methode, die angibt, wie die Komponente in das Projekt importiert werden sollen, wenn das Projekt erstellt wird. Dies kann eine der folgenden Werte sein:
    * **Kopieren:** Die Komponente ist von dem lokalen Pfad, indem Sie die Eigenschaft **aus** in die Rolle des **Approot** Verzeichnis angegeben kopiert.
    * **Halt:** Die Komponente ist ein Java Enterprise-Archiv (halt) importiert aus einem Enterprise-Anwendung-Projekt im lokalen Pfad angegeben haben, indem Sie die Eigenschaft **aus** . (Dies wird automatisch vom entsprechend der Art des Projekts an dieser Stelle Toolkit erkannt).
    * **JAR:** Die Komponente ist ein Java-Archiv (JAR) und importiert aus einem Projekt Java im lokalen Pfad angegeben haben, indem Sie die Eigenschaft **aus** . (Dies wird automatisch vom entsprechend der Art des Projekts an dieser Stelle Toolkit erkannt).
    * **keine:** So importieren Sie die Komponente keine Aktion ausgeführt. Dies ist verfügbar, wenn die Komponente davon ausgegangen, dass ist bereits in der Rolle des **Approot** Verzeichnis vorhanden sein, oder die Komponente ist lediglich eine ausführbare Befehlszeile-Anweisung, gemäß Angabe in der **als** Eigenschaft, wenn **die Methode** **Mitarbeiter**ist.
    * **WAR:** Die Komponente ist die Anwendung Java Webarchiv (WAR) und vom dynamische eines Projekts im lokalen Pfad angegeben haben, indem Sie die Eigenschaft **aus** importiert. (Dies wird automatisch vom entsprechend der Art des Projekts an dieser Stelle Toolkit erkannt).
    * **Zip:** Die Komponente ist eine Zipdatei und wird durch das Komprimieren von Verzeichnisses oder der Datei, die durch die Eigenschaft **aus** angegebenen importiert.
* **Aus:** Quellpfad auf dem lokalen Computer in den Ordner oder die Datei, die die Elemente, für die Bereitstellung zu importieren darstellt. Windows-Umgebungsvariablen können in dieser Eigenschaft verwendet werden. Die Rolle des **Approot** Verzeichnis werden alle Importierbare Komponenten importiert werden, wenn das Projekt erstellt wird.
    
    Beachten Sie, dass Sie die Möglichkeit haben, eine Komponente aus einem Download bereitstellen, wenn Sie in der Cloud (nicht die Serveremulator) bereitstellen. Zeigen Sie zugehöriger Informationen unter Informationen zum Hinzufügen einer Komponente an    
    
* **As:** Dateinamen ein, unter dem die Komponente in die Rolle des **Approot** Verzeichnis importiert wurden und schließlich in der Cloud Azure bereitgestellt werden. Lassen Sie diese Eigenschaft auf dem Namen der beibehalten, wie er auf dem lokalen Computer ist leer. (Ausführbare Komponenten, bei denen **Mitarbeiter**, dessen Methode **Bereitstellen** festgelegt ist, kann dies eine zufällige Windows Befehlszeile-Anweisung sein.)

    >[AZURE.IMPORTANT] Wenn Sie für diesen Wert mit Leerzeichen verwenden, werden diese je nach der Bereitstellungsmethode anders behandelt. Ist die Methode **Ausführen**, werden Leerzeichen als Trennzeichen verwendet Befehlszeile-Argument und nicht als Teil der Dateiname interpretiert. Bereitstellen von für alle anderen Methoden, Leerzeichen als Teil der Dateiname interpretiert.
    
* **Bereitstellen:** Methode, die zeigt an, die Aktion, die auf die Komponente angewendet wird, wenn die Bereitstellung gestartet wird. Dies kann eine der folgenden Werte sein:
    * **Kopieren:** Die Komponente ist von der Eigenschaft **zum** angegebenen Ziel Pfad kopiert.
    * **Mitarbeiter:** Die Komponente wird ausführbare Windows Befehlszeile Anweisung im Kontext des nach der Eigenschaft **,** die zum Zeitpunkt, den die Bereitstellung beginnt, angegebenen Pfads ausgeführt.
    * **keine:** Keine Aktion ist mit der Komponente angewendet, wenn die Bereitstellung startet.
    * **Zip:** Die Komponente ist von der Eigenschaft **zum** angegebenen Ziel Pfad extrahiert. Diese Methode steht nur, wenn Sie die Eigenschaft **Importieren** **Zip**ist.
* **To:** Den Zielpfad des virtuellen Computers, auf dem die Komponente bereitgestellt wird. Windows-Umgebungsvariablen in dieser Eigenschaft verwendet werden können, und Dateipfade relativ zur **Approot**.
    
Um eine neue Komponente hinzuzufügen, klicken Sie auf die Schaltfläche " **Hinzufügen** " in der Eigenschaftenseite **Komponenten** , und ein Dialogfeld **Azure Rolle Komponente** wird geöffnet. Geben Sie Werte für die Eigenschaften für die oben beschrieben werden. 

Die nachstehende Abbildung zeigt ein Beispiel für eine neue WAR-Komponente hinzufügen.

![][ic719503]

Bei der Bereitstellung in der Cloud (nicht die Serveremulator), wenn Sie die Komponente aus einem Download bereitstellen möchten, stellen Sie sicher, dass **in der Cloud, statt in das Paket einschließlich, Bereitstellen von** aktiviert ist. Wenn Sie über Ihr Konto Azure-Speicher, wählen Sie die Speicherung Konto aus der **Speicherkonto** Dropdown-Liste (Sie können **Konten** auf den Link Klicken ändern, was in der Liste enthalten ist), die teilweise in das Feld **URL** füllen wird, und füllen Sie die restlichen Teil der URL herunterladen möchten. Wenn Sie nicht Azure-Speicher verwenden möchten, wählen Sie **(keine)** aus der Dropdownliste **Speicher-Konto** , und geben Sie an die Komponente in das Feld **URL** die URL. Geben Sie eine der folgenden Methoden:

* **Kopieren:** Die Downloadkomponente wird durch den Pfad **Zum Verzeichnis** angegebenen Pfad Ziel kopiert.
* **gleichen:** Die gleiche Methode zum **Bereitstellen von Download** wie beim **Bereitstellen von Paket**verwendet.
* **Zip:** Die Downloadkomponente ist durch den Pfad **Zum Verzeichnis** angegebenen Pfad Ziel extrahiert.

Um eine Komponente zu ändern, wählen Sie die Komponente aus, und klicken Sie auf die Schaltfläche **Bearbeiten** in der Eigenschaftenseite **Komponenten** . Ein Dialogfeld wird ermöglicht es Ihnen, ändern die Eigenschaften der geöffnet. Drücken Sie **OK** , um die Komponentenwerte zu speichern.

Wählen Sie die Komponente um eine Komponente zu löschen, klicken Sie auf die Schaltfläche " **Entfernen** " in der Eigenschaftenseite **Komponenten** , und klicken Sie dann auf **Ja,** um den Löschvorgang zu bestätigen.

Komponenten werden in der aufgeführten Reihenfolge ausgeführt. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten** , um die Reihenfolge anzuordnen.

>[AZURE.NOTE] Die Server-Konfigurations-Funktion basiert auf Komponenten ebenfalls. Diese Komponenten nicht entfernt oder bearbeitet werden, ohne die entsprechenden Server-Konfiguration entfernen. Sie werden über die aufgefordert, wenn Sie versuchen, solche Komponenten ändern.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Für das Debuggen Eigenschaften ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, klicken Sie auf **Azure**, und klicken Sie dann auf **Debuggen**. In diesem Dialogfeld Sie haben die Möglichkeit zum Aktivieren oder Deaktivieren von remote Debuggen sowie debuggen möchten, erstellen, wie in der folgenden Abbildung gezeigt.

![][ic719504]

Verwandte Informationen über das Debuggen finden Sie unter [Debuggen Azure Applications in "Ellipse"][].

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Endpunkte Eigenschaften ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich auf **Azure**, und klicken Sie dann auf **Endpunkte**. In diesem Dialogfeld haben Sie die Möglichkeit zum Erstellen von außen liegenden Tabellenblättern als auch bearbeiten oder Entfernen von außen liegenden Tabellenblättern, wie in der folgenden Abbildung gezeigt.

![][ic719505]

Um einen Endpunkt hinzufügen möchten, klicken Sie auf die Schaltfläche **Hinzufügen** , in die **Endpunkte** Eigenschaftenseite und ein Dialogfeld **Endpunkt hinzufügen** wird geöffnet.

![][ic710897]

Geben Sie einen Namen für den Endpunkt, wählen Sie den Typ ( **Eingabe**, **intern**, oder **InstanceInput**), und geben Sie den öffentlichen und privaten Port. Drücken Sie **OK** , um die neuen Endpunkt Werte zu speichern.

Je nach Art der Endpunkt können Sie Anschlussbereiche wie folgt verwenden:

* Für einen Instanzendpunkt einer kann der öffentliche Port einen Bereich von Ports (beispielsweise **2000-2010**) und der private Anschluss ist ein fester Wert.
* Bei einem internen Endpunkt kann der öffentliche Port nicht verwendet wird, und der private Anschluss einen Bereich, oder links leer oder festlegen, um ein Sternchen an, dass es wird automatisch durch Azure festgelegt.
* Für Eingabewerte Endpunkte der öffentliche Port kann nur ein festes Wert, und der private Anschluss kann festen Wert oder links leer sein oder auf das Sternchen festlegen, um anzugeben, dass er automatisch vom Azure festgelegt wurde.

Wenn Sie eine einzelne Port-Nummer anstelle eines Bereichs verwenden möchten, lassen Sie das Textfeld für das Ende der Bereich leer.

Für Ports, die auf automatische, wenn Sie festgelegt werden müssen, um festzustellen, welchen Anschluss Laufzeit tatsächlich verwendet wird, kann die Anwendung der Azure Service Runtime-API verwenden die im [com.microsoft.windowsazure.serviceruntime Paket Zusammenfassung][]beschrieben ist.

Um anzuzeigen, wie die Instanz von Endpunkten unterstützen Sie beim Debuggen einer bereitstellungs mit mehreren Instanzen verwendet werden können, finden Sie unter [Debuggen einer bestimmten Rolleninstanz in einer Bereitstellung mit mehreren Instanzen][].

Wenn Sie einen Endpunkt ändern möchten, wählen Sie den Endpunkt, und klicken Sie auf die Schaltfläche **Bearbeiten** in die Eigenschaftenseite **Endpunkte** . Ein Dialogfeld wird ermöglicht es Ihnen, den Endpunktname und Typ öffentlichen und privaten Ports ändern geöffnet. Drücken Sie **OK** , um die geänderte Endpunkt Werte zu speichern.

Zum Löschen von außen liegenden Tabellenblättern wählen Sie den Endpunkt und klicken Sie auf die Schaltfläche " **Entfernen** " in die **Endpunkte** Eigenschaftenseite, und klicken Sie dann auf **Ja,** um den Löschvorgang zu bestätigen.

Um einige der Features (z. B. zwischenspeichern, Remotedebuggen, Sitzung Zugehörigkeit oder SSL Verschiebung) von Benutzern auf eine Rolle aktiviert ordnungsgemäß zu konfigurieren, kann das Toolkit automatisch Inhalte Endpunkte konfigurieren, die zusammen mit User defined Endpunkte aufgelistet werden. Das Toolkit verhindert, dass des Benutzers bearbeiten oder Endpunkte löschen beispielsweise automatisch generiert werden, solange die zugeordnete Funktion aktiviert ist.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Eigenschaften der Umgebung Variablen ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich auf **Azure**, und klicken Sie dann auf **Variablen Umgebung**. In diesem Dialogfeld haben Sie die Möglichkeit, erstellen Sie eine Umgebungsvariable-, die als auch ändern oder Entfernen einer Umgebungsvariable wie in der folgenden Abbildung gezeigt.

![][ic719506]

Umgebungsvariablen sind für Ihre Skript zum Starten des verfügbar, wenn die Rolle gestartet wird.

>[AZURE.NOTE] Beachten Sie bei der Umgebungsvariablen zurück, der angibt, beachten Sie, dass die Bereitstellung auf einem Windows-Computer, veröffentlicht wird, damit Ihre Umgebungsvariablen für ein Betriebssystem Windows-basierten gültigen werden müssen.

Als ein Beispiel für eine Umgebungsvariable nicht verfügbar sein, wenn die Rolle gestartet wird erstellen Sie eine neue Umgebungsvariable durch Klicken auf die Schaltfläche **Hinzufügen** . Die nachstehende Abbildung zeigt eine Umgebungsvariable mit dem Namen **MyRoleVersion** erstellt wird und der Wert **1.0**zugewiesen.

![][ic659268]

Innerhalb des Codes Jsp konnte Anzeigen der Wert mit dem `System.getenv` Methode:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

In dieser Ausgabe resultierender, wenn die Anwendung ausgeführt wird:

![][ic552233]

Um eine Umgebungsvariable zu ändern, wählen Sie die Umgebungsvariable aus und klicken Sie auf die Schaltfläche **Bearbeiten** in der **Umgebungsvariablen** Eigenschaftenseite. Ein Dialogfeld wird ermöglicht es Ihnen, ändern Sie die Variable Umgebung-Eigenschaften geöffnet. Drücken Sie **OK** , um die Umgebung Variablenwerten speichern aus.

Um eine Umgebungsvariable zu löschen, wählen Sie die Umgebungsvariable klicken Sie auf die Schaltfläche " **Entfernen** " in der **Umgebungsvariablen** Eigenschaftenseite, und klicken Sie dann auf **Ja,** um den Löschvorgang zu bestätigen.

Um einige der Features (z. B. Server-Konfiguration, Remotedebuggen oder lokaler Speicher) von Benutzern auf eine Rolle aktiviert ordnungsgemäß zu konfigurieren, möglicherweise das Toolkit automatisch Umgebungsvariablen spezielle konfigurieren, die zusammen mit der User defined Umgebungsvariablen aufgelistet werden. Das Toolkit verhindert, dass des Benutzers bearbeiten oder Umgebungsvariablen löschen beispielsweise automatisch generiert werden, solange die zugeordnete Funktion aktiviert ist.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Lastenausgleich / Sitzung Zugehörigkeit (bzw. "Kurznotizen Sitzungen") Eigenschaften ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, klicken Sie auf **Azure**, und klicken Sie dann auf **Den Lastenausgleich**. In diesem Dialogfeld haben Sie die Möglichkeit zum Aktivieren oder deaktivieren die Sitzung Zugehörigkeit, wie in der folgenden Abbildung gezeigt.

![][ic719492]

Weitere Informationen finden Sie unter [Sitzung Zugehörigkeit][]. Beachten Sie auch, des Features Verhalten im Zusammenhang mit SSL Verschiebung, wie bei [SSL Verschiebung][]beschrieben.

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Speichereigenschaften der lokalen ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich auf **Azure**, und klicken Sie dann auf **Lokale Speicher**. In diesem Dialogfeld haben Sie die Möglichkeit zum Erstellen, ändern oder Entfernen von temporären lokalen Speicher für den virtuellen Computern, der die Anwendung ausgeführt wird. Bestimmte Werte können für die Größe der lokalen Speicher festgelegt werden sowie, ob der Inhalt beibehalten werden, wenn die Rolle des wiederverwendet, ist, wie in der folgenden Abbildung gezeigt.

![][ic719508]

Sie können auch optional ON angeben, die die lokale Speicherung entspricht.

Standardmäßig ist alle Elemente, die Sie in Azure bereitstellen platziert (in den Ordner **Approot** , der die Instanz der Rolle und entpackt haben). Während werden von den meisten einfache Bereitstellungen ist es passt gerade nach Entzippen, des für das Verzeichnis **Approot** Speicherplatzes und beschränkt nicht klar definiert (weniger als 1 GB angemessenen Faustregel ist). Um sicherzustellen, dass Azure genügend Speicherplatz für größere Bereitstellungen zuweist, die nicht in den Ordner **Approot** abgelegt werden kann, sollten Sie daher, richten Sie eine lokale Speicherressource mithilfe des Dialogfelds **Lokaler Speicher** festlegen. Für eine einfache Möglichkeit hierzu finden Sie unter [Bereitstellen von großen Bereitstellungen][].

Sie können einfach die Speicherressource von Skripts zum starten möchten (beispielsweise Ihre **startup.cmd**) mithilfe der Umgebungsvariable automatisch vom Ellipse Toolkit der Ressource zugeordnet, wie im Dialogfeld **Lokale Speicher** dargestellt verweisen. Diese Umgebungsvariable wird den vollständigen Pfad der lokalen Ressource enthalten, die Sie zum Zeitpunkt konfiguriert haben, wenn, die Ihr Skript zum Starten des ausgeführt wird. 

Um eine lokale Speicherressource zu ändern, wählen Sie die lokale Speicherressource, und klicken Sie auf die Schaltfläche **Bearbeiten** in der **Lokalen Speicher** Eigenschaftenseite. Ein Dialogfeld wird ermöglicht es Ihnen, ändern Sie die Eigenschaften der lokalen Speicher Ressource geöffnet. Drücken Sie **OK** , um die lokale Speicher Ressourcenwerte zu speichern.

Zum Löschen einer lokalen Speicherressource wählen Sie die Ressource lokaler Speicher klicken Sie auf die Schaltfläche " **Entfernen** " in der **Lokalen Speicher** Eigenschaftenseite, und klicken Sie dann auf **Ja,** um den Löschvorgang zu bestätigen.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Eigenschaften der Server-Konfiguration ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich, klicken Sie auf **Azure**, und klicken Sie dann auf **Server-Konfiguration**. In diesem Dialogfeld haben die Möglichkeit zum Hinzufügen, entfernen und ändern den von der Bereitstellung, verwendeten Anwendungsserver JDK und Java Sie als auch hinzufügen oder entfernen die Clientanwendungen (z. B. WAR, JAR- oder Halt-Dateien) von der Bereitstellung verwendet werden.

### <a name="jdk-configuration"></a>JDK-Konfiguration ###

In diesem Dialogfeld können Sie angeben, das Paket JDK für die Bereitstellung verwendet werden soll. Wenn Sie unter Windows "Ellipse" verwenden, Sie können angeben, dass das Paket JDK lokal verwenden, wenn der Azure Emulator ausgeführt, und Sie haben die Möglichkeit, die lokale Installation auf Azure bereitstellen. Klicken Sie auf nicht Windows-Betriebssysteme die Emulator JDK-Einstellung ist nicht verfügbar und lokal installierte JDK kann nicht bereitgestellt werden, da es nicht mit Windows kompatibel ist. Jedoch können unabhängig vom Betriebssystem, das Sie verwenden, immer die Auswahl zwischen den 3rd Party JDK-Paketen in Azure bereitstellen, oder zeigen Sie auf Ihrer eigenen Windows-kompatiblen JDK-Paket aus einer alternativen Downloadspeicherort.

Im folgenden finden ein Beispiel, wie Sie eine JDK auf Windows angeben können:

![][ic780647]

Wenn Sie unter Windows "Ellipse" verwenden, können Sie eine JDK zur Verwendung mit der Serveremulator angeben; Hierzu stellen Sie sicher, dass im Abschnitt **Emulator Bereitstellung** **verwenden JDK aus dieser Dateipfad für Lokales Testen** aktiviert ist. Klicken Sie dann den lokalen Pfad zu Ihrem JDK anzugeben. Sie können zu anderen JDK navigieren, wenn das Element, das Sie verwenden möchten, nicht automatisch aktiviert ist. Sie haben auch die Möglichkeit, Ihre JDK an den Azure-Cloud-Dienst bereitstellen; Wählen Sie hierzu die Option **Bereitstellen meinem lokalen JDK (Auto-Upload in einen Cloud-Speicher)** im Abschnitt **Cloud-Bereitstellung** ein.

Hinweis: Klicken Sie auf nicht-Windows-Betriebssysteme sind die Einstellungen für die **Bereitstellung der Emulator** und anschließend die Option **Bereitstellen meinem lokalen JDK** nicht verfügbar. Im folgende Beispiel wird veranschaulicht, eine JDK auf einem Mac oder andere unterstützte nicht Windows-Betriebssystem angeben:

![][ic789643]

Unabhängig vom Betriebssystem, die, dem Sie sich befinden, müssen Sie die folgenden zwei **Cloud** Bereitstellungsoptionen für Quell- und Typ von Ihrem JDK-Paket aus:

* **Bereitstellen eines 3rd Party JDK Pakets auf Azure zur Verfügung** 
* **Bereitstellen von benutzerdefinierten download** 

Wenn Sie die Option **Bereitstellen einer 3rd JDK Paket aus Azure party** verwenden:

1. Aktivieren Sie das Kontrollkästchen mit dem Namen **eines 3rd JDK Paket aus Azure party bereitstellen**.
1. Wählen Sie aus der Dropdownliste 3rd Party JDK Paket, das auf Azure zur Verfügung.
1. Ihre **JDK** Registerkarte aussieht wie die folgende unter Windows:  ![][ic780648] 
    und es sieht in etwa wie folgt unter Mac OS oder anderen nicht-Windows-Betriebssystemen unterstützt: ![][ic789643]
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.
1. Wenn Sie aufgefordert werden, den vom 3rd Party JDK Paket Anbieter-Lizenzvertrag anzunehmen, überprüfen Sie die Lizenzbedingungen an. Vorausgesetzt, dass Sie zustimmen, klicken Sie auf **Ja,** um das Dialogfeld **Lizenzvertrag akzeptieren** schließen.
    Beachten Sie, dass die zugrunde liegende Logik für die Elemente, in der Dropdownliste die Option **Bereitstellen einer 3rd JDK Paket aus Azure party angezeigt** angepasst werden kann. Klicken Sie auf den Link **Anpassen** , um die Elemente aus, klicken Sie im Dialogfeld **JDK** anzupassen. Dadurch wird die Eigenschaftenseite **JDK** schließen und öffnen die Datei **componentsets.xml** in "Ellipse", die Sie dann je nach Bedarf ändern können. Dokumentation für **componentsets.xml** ist in der **componentsets.xml** -Datei selbst enthalten.

Wenn Sie die Option **Bereitstellen einer JDK aus einer benutzerdefinierten Download** verwenden:

1. Erstellen eines ZIP von Ihrem JDK Installationsverzeichnis, um sicherzustellen, dass der Directory-Knoten selbst untergeordnet ZIP-Struktur und nicht auf deren Inhalt ist. Notieren Sie den Namen des Verzeichnisses, während Sie diese später benötigen, und denken Sie daran, die diese JDK-Installation in einem Windows-Computer bereitgestellt werden soll.
1. Hochladen der ZIP in Ihr Konto Azure-Speicher als Blob. Sie können dazu eines extern verfügbaren Tools für Hochladen von Blobs zu Azure-Speicher. Es wird empfohlen, einen privaten Blob verwenden. Notieren Sie sich die Blob-URL, der den Inhalt von ZIP.
1. Aktivieren Sie das Kontrollkästchen mit dem Namen **Bereitstellen einer JDK aus einer benutzerdefinierten herunterladen**.
    Wenn Sie über Ihr Konto Azure-Speicher, wählen Sie die Speicherung Konto aus der **Speicherkonto** Dropdown-Liste (Sie können **Konten** auf den Link Klicken ändern, was in der Liste enthalten ist), die teilweise in das Feld **URL** füllen wird, und füllen Sie die restlichen Teil der URL herunterladen möchten. Wenn Sie nicht Azure-Speicher verwenden möchten, wählen Sie **(keine)** aus der Dropdownliste **Speicher-Konto** , und geben Sie Ihre JDK Download in das Feld **URL** die URL. Wenn Azure-Speicher verwenden zu können, muss Blob-Namen in der URL Kleinbuchstaben.
1. Stellen Sie sicher, dass das Textfeld **JAVA_HOME** auf den Namen der richtigen Verzeichnis verweist. Standardmäßig wird den gleichen JDK Verzeichnisnamen als Wert verwiesen werden, die Sie für Ihre lokale verwenden ausgewählt haben. Aber weist das im ZIP-Verzeichnis auf einen anderen Namen (beispielsweise aufgrund einer anderen Version), der Name des Verzeichnisses in das Textfeld **JAVA_HOME** entsprechend aktualisieren, da diese Einstellung in der Cloud (nicht in der Serveremulator) verwendet wird.
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Das war's auch. Jetzt, wenn Sie für die Cloud erstellen, sehen Sie die Paketgröße werden viel kleiner, der erstellen sollten in der Regel dauern weniger Zeit, und der bereitstellungs, selbst wenn Sie in der Cloud veröffentlichen sollte auch weniger Zeit. Beachten Sie, dass die Optionen **Bereitstellen meinem lokalen JDK (Auto-Upload in einen Cloud-Speicher)** oder **Bereitstellen einer JDK aus einer benutzerdefinierten Download** wirksam sind nur, wenn die Anwendung in der Cloud bereitgestellt wird. Sie haben keine Auswirkung auf Ihre berechnen Emulator Erfahrungen; die lokale Version der Komponenten wird weiterhin verwendet werden, wenn Sie die Serveremulator bereitstellen. 

### <a name="server-configuration"></a>Server-Konfiguration ###

So sieht ein Beispiel wie Sie einen Anwendungsserver angeben können.

![][ic796926]

Stellen Sie sicher, dass das Kontrollkästchen **Bereitstellen einer Server dieses Typs** ausgewählt ist, und wählen Sie dann auf den Typ der Anwendungsserver, die Sie verwenden möchten.

Für einen Server angeben, für die Cloudbereitstellung verwenden, können Sie die Nutzen der folgenden Optionen:

1. **Bereitstellen einer 3rd Server verfügbar auf Azure party** – Dies ist insbesondere in Test-/Szenarien, wo Bereitstellungseffizienz und Vereinfachung ist eine Priorität und der Server erfordert eine benutzerdefinierte Konfiguration nicht, verfügbar. Oder wenn Sie eine dieser Server als Ausgangspunkt verwenden möchten, aber Sie aufnehmen Schritte Anpassung der entsprechenden Server in der Bereitstellung Startlogik.
1. **Bereitstellen von benutzerdefinierten Download** – Dies ist besonders in Herstellung Szenarien verfügbar, wenn Sie einen Server speziell vorbereiteten und konfigurierten haben, die Sie in der Cloud verwenden möchten.
1. **Bereitstellen von meinem lokalen Serverinstallation** – Dies ist besonders in verfügbar, wenn die lokale Server-Installation noch benutzerdefinierte für Ihre Verwendung konfiguriert ist. Wenn Sie diese Option auswählen, müssen Sie Ihrem lokalen Server Pfad auch in das Textfeld **lokalen Serverpfad** unten angeben.

Wenn Sie die Option **Bereitstellen einer 3rd Server verfügbar auf Azure party** verwenden:

1. Aktivieren Sie das Kontrollkästchen mit dem Namen **eines 3rd Server verfügbar auf Azure party bereitstellen**.
1. Wählen Sie die gewünschten Serversoftware zur Verwendung mit der Bereitstellung in der Cloud aus dem Dropdownmenü aus. Beachten Sie: Wenn Sie einen Typ des Servers, mit einer früheren bereits angegeben haben, werden Sie zum Auswählen von nur eines Cloud-Servers, der in der gleichen Familie Dateityp die Server ist, eingeschränkt. Aber wenn Sie ein anderes Server nicht ausgewählt haben, können Sie von jedem der Server, die derzeit auf Azure verfügbar sind auswählen und Typ des Servers werden automatisch für Sie aktiviert.
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Wenn die Option **Bereitstellen von benutzerdefinierten Download** verwenden zu können:

1. Stellen Sie sicher, dass Sie bereits eine Server entsprechend den vorherigen Schritten ausgewählt haben. Dies weist dem Plug-in zur Bereitstellung des Servers aus Ihrer benutzerdefinierten herunterladen, wie sie in der gleichen Familie Dateityp der ausgewählten Server sein sollte.
1. Aktivieren Sie das Kontrollkästchen mit dem Namen **aus einer benutzerdefinierten Download bereitstellen**.
    Wenn Sie über Ihr Konto Azure-Speicher herunterladen möchten, herunterladen auswählen die Speicherung Kontonamens aus der **Speicherkonto** Dropdown-Liste (Sie können **Konten** auf den Link Klicken ändern, was in der Liste enthalten ist), die sich teilweise in das Feld **URL** ausfüllt sowie und füllen Sie die restlichen Teil der URL zu Ihrem Server ZIP (wenn mit Azure-Speicher, Blob-Namen in der URL Kleinbuchstaben sein muss). Wenn Sie nicht Azure-Speicher verwenden möchten, wählen Sie **(keine)** aus der Dropdownliste **Speicher-Konto** , und geben Sie die URL zu Ihrem Server-Download ZIP in das Feld **URL** . Die ZIP würde einen untergeordneten Ordner, die Ihrer Anwendung Serverinstallationsverzeichnis darstellt enthalten. Angenommen, bei der Verwendung eines Zip für wäre Apache Tomcat 7.0.35, innerhalb der Zip der untergeordneten Ordner, die das Installationsverzeichnis, z. B. **Apache-Tomcat-7.0.35**darstellt. 
1. Geben Sie den Wert für die home-Verzeichnis Umgebungsvariable ein. Es wird standardmäßig auf den Wert für Ihre lokale Anwendungsserver, verwendet wird, sofern vorhanden, aber Sie können einen anderen Wert angeben, wenn Ihre Cloud Anwendungsserver aus Ihrem lokalen Anwendungsserver unterscheidet. Jedoch müssen Sie sicherstellen, dass Ihre Cloud Anwendungsserver derselben Familie wie der Server ist zuvor ausgewählten Typ.
    Wenn Sie Ihre Cloud-Anwendung Server Postleitzahl zukünftig aktualisieren, können Sie die Einstellung Privat-Ordner manuell ändern oder so, dass sie erneut entsprechen Ihre lokale Einstellung (Wenn Sie zu Ihrem lokalen Anwendungsserver geändert).
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Die zugrunde liegende Logik für die Elemente auf der Registerkarte **Server** die Eigenschaftenseite **Server-Konfiguration** angezeigt werden kann angepasst werden. Dies ist eine erweiterte Funktion, die Sie möglicherweise benötigen, wenn Sie Ihren Anforderungen hinausgehen, die Standardwerte oder andere Server hinzugefügt werden sollen. Klicken Sie zum Anpassen der Logik, klicken Sie im Dialogfeld **Server** auf den Link **Anpassen** . Dies wird die **Konfiguration des Servers** Eigenschaftenseite schließen und öffnen die Datei **componentsets.xml** in "Ellipse", die Sie dann ändern können, je nach Bedarf, um die Server-Konfigurations-Vorlage zu erweitern. Dokumentation für **componentsets.xml** ist in der **componentsets.xml** -Datei selbst enthalten.

Wenn Sie die Option **Bereitstellen meinem lokalen Server (Auto-Upload in einen Cloud-Speicher)** verwenden:

1. Aktivieren Sie das Kontrollkästchen mit dem Namen **Bereitstellen meinem lokalen Server (Auto-Upload in einen Cloud-Speicher)**.
1. Wählen Sie mit der Dropdownliste **Speicher-Konto** **(Auto)**. Wenn Sie hier **(Auto)** angeben, wird das Ellipse Toolkit dasselbe Speicherkonto für Ihren Server als verwendet, die Sie für die Bereitstellung im Dialogfeld **Veröffentlichen in Azure** auswählen.
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Wählen Sie im Textfeld **Pfad auf dem lokalen Server** einen Server Installationspfad auf Ihrem Computer aus, wenn eine der folgenden Bedingungen zutrifft:

* Sie möchten die Bereitstellung im Emulator testen (gilt nur für Windows).
* Möchten Sie Ihre Server lokal installierten in der Cloud bereitstellen.
* Verwenden eines benutzerdefinierten Server-Downloads für eigene in der Cloud, in dem Fall Sie außerdem sicherstellen, dass die Option **Bereitstellen meinem lokalen Server (Auto-Upload in einen Cloud-Speicher)** ausgewählt ist, über aufheben möchten.

Wenn keine der obigen Optionen auf Ihre Situation zutrifft, ist die Einstellung lokalen Server optional.

### <a name="applications-configuration"></a>Konfiguration von Applications ###

So sieht ein Beispiel, wie Sie eine Anwendung angeben können.

![][ic719512]

Klicken Sie auf **Hinzufügen** zu einer anderen Anwendung hinzufügen oder **Entfernen** , um eine Anwendung zu entfernen. Aus Gründen der Effizienz, wenn Sie einen Download für die Quelle der Anwendung zu verwenden, wenn Sie in der Cloud bereitstellen möchten, verwenden Sie die [Komponenten Eigenschaften](#components_properties) einer URL, usw. Speicher-Konto an. 

Beginnen mit der Veröffentlichung April 2014, werden Ihre Programme automatisch in der gleichen Speicherkonto (unter den Container **Eclipsedeploy** ) als für die Bereitstellung hochgeladen. Die Startlogik der Bereitstellung enthält einen Schritt, der Sie zunächst diese Applications aus diesem Speicherkonto herunterladen. Dies bedeutet, dass Sie möglicherweise in der Bereitstellung Ihrer Anwendung aktualisieren, ohne neu erstellen und erneut das gesamte Paket, indem Sie manuell hochladen neuere Versionen der Anwendung direkt in diesem Storage-Konto (z. B. mit dem Azure-Portal) bereitstellen, ersetzen die WAR-Dateien ursprünglich es vom Toolkit hochgeladen. Klicken Sie dann einfach initiieren Sie die Wiederverwendung von alle Rolleninstanzen mithilfe des Azure-Verwaltungsportal von erneut oder über die Befehlszeilendienstprogramme. (Direkt in die Ellipse Toolkit Wiederverwendung Rolle auslösen wird nicht aktuell unterstützt.)

### <a name="notes-about-server-configuration"></a>Hinweise zu Server-Konfiguration ###

Durch die **Konfiguration des Servers** Eigenschaftenseite vorgenommene Änderungen werden angezeigt, der `<component>` Elemente der Datei package.xml.

Wenn Sie die **automatisch hochladen...** verwenden oder **Bereitstellen von Download...** Optionen für die JDK oder Anwendungsserver, und Sie für die Cloud (nicht die Serveremulator) erstellen, und Sie mit dem Netzwerk verbunden sind, möglicherweise stellen Sie fest Nachrichten, beispielsweise die folgende in der Ausgabe Console erstellen während des Ant-Generators Verfügbarkeit der Download des überprüft:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Wenn Sie die Option **Bereitstellen aus... Download** ausgewählt haben, kann die folgende Warnung angezeigt werden, aber weiterhin das Erstellen:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Diese Warnung wird nur angezeigt, dass das Herunterladen der Verfügbarkeit noch nicht überprüft wurden. Ja, wenn eine Bereitstellung in der Cloud aus irgendeinem Grund fehlschlägt, überprüfen Sie angezeigt, wenn Sie diese Warnung erhalten haben.

Wenn Sie möchten, deaktivieren die Überprüfung herunterladen (beispielsweise, wenn Sie den Eindruck verlangsamt es unnötig Build), legen Sie die `verifydownloads` Attribut zu `false` in der `<windowsazurepackage>` Element des package.xml: 

`<windowsazurepackage verifydownloads="false" ...>` 

Wenn Sie entschieden, die **automatisch hochladen...** , klicken Sie dann in der Konsole, die angezeigt werden, ist erstellen Nachrichten dargestellt wird der Status des Uploads 5 Sekunden, wenn ein Upload erforderlich, vor.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL-Verschiebung Eigenschaften ###

Öffnen Sie das Kontextmenü für die Rolle des "Ellipse" Projekt-Explorer-Bereich auf **Azure**, und klicken Sie dann auf **SSL Verschiebung**. 

![][ic719481]

In diesem Dialogfeld können Sie aktivieren SSL Verschiebung, ermöglicht es Ihnen, Hypertext Transfer Protocol Secure (HTTPS) Unterstützung in der Java-Bereitstellung auf Azure einfach zu aktivieren, ohne dass Sie Ihre Java-Anwendungsserver SSL konfigurieren. Weitere Informationen finden Sie unter [SSL Verschiebung][] und [wie Sie verwenden SSL Verschiebung][].

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][]

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

[Azure Projekteigenschaften][]

[Liste der Azure-Speicher-Konto][]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Projekteigenschaften]: http://go.microsoft.com/fwlink/?LinkID=699524
[Liste der Azure-Speicher-Konto]: http://go.microsoft.com/fwlink/?LinkID=699528
[Zusammenfassung com.Microsoft.windowsazure.ServiceRuntime-Paket]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Eine Instanz der jeweiligen Rolle in einer Bereitstellung mit mehreren Instanzen Debuggen]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Für das Debuggen Azure Applications in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699535
[Bereitstellen von großen Bereitstellungen]: http://go.microsoft.com/fwlink/?LinkID=699536
[So verwenden Sie am selben Standort Zwischenspeichern]: http://go.microsoft.com/fwlink/?LinkID=699542
[So verwenden Sie SSL Verschiebung]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546
[Sitzung Zugehörigkeit]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-Verschiebung]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
