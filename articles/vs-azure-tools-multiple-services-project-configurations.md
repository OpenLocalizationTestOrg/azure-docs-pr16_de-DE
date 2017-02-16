<properties
   pageTitle="Konfigurieren von Azure Projekt über mehrere Dienstkonfigurationen | Microsoft Azure"
   description="Erfahren Sie, wie ein Azure-Cloud-Service-Projekt konfigurieren, indem Sie die Dateien ServiceDefinition.csdef und ServiceConfiguration.cscfg ändern."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurieren von Azure Projekt über mehrere Dienstkonfigurationen

Ein Azure-Cloud-Service-Projekt umfasst zwei Konfigurationsdateien: ServiceDefinition.csdef und ServiceConfiguration.cscfg. Diese Dateien werden mit der Azure-Cloud-Service-Anwendung verpackt und auf Azure bereitgestellt.

- Die **ServiceDefinition.csdef** -Datei enthält die Metadaten, die von der Azure-Umgebung für den Anforderungen der Cloud Service-Anwendung, welche Rollen enthaltenen einschließlich erforderlich ist. Diese Datei enthält auch Konfiguration Einstellungen, die für alle Instanzen gelten. Diese Einstellungen Konfiguration können zur Laufzeit mithilfe von Azure Service Hosting Runtime-API gelesen werden. Diese Datei kann nicht aktualisiert werden, während des Diensts in Azure ausgeführt wird.

- Die Datei **ServiceConfiguration.cscfg** Werte für die Konfiguration Einstellungen in der Definition Dienstdatei definiert und gibt die Anzahl der Instanzen für jede Rolle ausführen. Während der Cloud-Dienst in Azure ausgeführt wird, kann diese Datei aktualisiert werden.

Azure-Tools für Microsoft Visual Studio bereitstellen Eigenschaftenseiten, die Sie verwenden können, Konfiguration Einstellungen in diesen Dateien gespeichert. Für den Zugriff auf die Eigenschaftenseiten Doppelklicken Sie auf den Bezug Rolle unter das Projekt Azure-Cloud-Dienst in Lösung Explorer oder mit der rechten Maustaste in des Bezug Rolle aus, und wählen Sie **Eigenschaften**aus, wie in der folgenden Abbildung gezeigt.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informationen über die zugrunde liegenden Schemas für den Dienst und die Dateien für Dienst Konfiguration finden Sie unter [Schema verweisen](https://msdn.microsoft.com/library/azure/dd179398.aspx). Weitere Informationen zu Dienstkonfiguration finden Sie unter [So konfigurieren Sie Cloud Services](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurieren der Rolleneigenschaften für

Die Eigenschaftenseiten einer Webrolle und eine Worker-Rolle ähneln, obwohl es einige Unterschiede, die in den folgenden Abschnitten darauf hingewiesen gibt.

Von der Seite **Zwischenspeichern** können Sie die Services Zwischenspeichern Azure konfigurieren.

### <a name="configuration-page"></a>Seite "Konfiguration"

Klicken Sie auf der Seite **Konfiguration** können Sie diese Eigenschaften festlegen:

**Instanzen**

Legen Sie die **Instanz** Count-Eigenschaft auf die Anzahl der Instanzen, die für diese Rolle im Dienst ausgeführt werden soll.

Legen Sie die Eigenschaft **virtueller Speicher** auf **Zusätzliche klein**, **klein**, **Mittel**, **Groß**oder **Sehr groß**.  Weitere Informationen finden Sie unter [Größen für Cloud-Dienste](./cloud-services/cloud-services-sizes-specs.md).

**Startaktion beim** (Nur Webrolle)

Legen Sie diese Eigenschaft, um anzugeben, dass Visual Studio einen Webbrowser für die HTTP-Endpunkte oder die Endpunkte HTTPS oder beide starten sollte, wenn Sie das Debuggen starten.

Die HTTPS-Endpunkt-Option ist nur verfügbar, wenn Sie bereits einen HTTPS-Endpunkt für Ihre Rolle definiert haben. Sie können einen HTTPS-Endpunkt auf der Eigenschaftenseite **Endpunkte** definieren.

Wenn Sie einen HTTPS-Endpunkt bereits hinzugefügt haben, die Option HTTPS-Endpunkt ist standardmäßig aktiviert, und Visual Studio wird beim Starten von Debuggen, über einen Browser für Ihre HTTP-Endpunkt einen Browser für diesen Endpunkt zu starten. Dies setzt voraus, dass sowohl Startoptionen aktiviert sind.

**Diagnose**

Standardmäßig ist die Diagnose für die Web-Rolle aktiviert. Das Azure-Cloud-Projekt und Speicher Dienstkonto festgelegt sind, dass die lokale Speicheremulator verwenden. Wenn Sie für die Bereitstellung auf Azure bereit sind, können Sie die Schaltfläche Generator****(...), aktualisieren Sie das Speicherkonto um Azure-Speicher in der Cloud zu verwenden auswählen. Sie können bei Bedarf oder regelmäßig automatisch die Diagnosedaten mit dem Speicherkonto übertragen. Weitere Informationen zur Azure-Diagnose finden Sie unter [Aktivieren der Diagnose in Azure-Cloud-Diensten und virtuellen Computern](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Einstellungsseite

Klicken Sie auf **der Einstellungsseite** können Sie Einstellungen für die Konfiguration des Diensts hinzufügen. Konfiguration von Einstellungen sind Name / Wert-Paare transformieren. Code, der in der Rolle ausgeführt, kann die Werte der Konfiguration Einstellungen zur Laufzeit mithilfe der [Azure verwaltete Bibliothek](http://go.microsoft.com/fwlink?LinkID=171026)Klassen lesen. Die Methode [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) gibt insbesondere den Wert einer benannten Konfiguration Einstellung zur Laufzeit.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfigurieren einer Verbindungszeichenfolge für Speicher-Konto

Eine Verbindungszeichenfolge ist eine Einstellung für die Konfiguration, die Verbindung und Authentifizierungsinformationen für Speicheremulator oder für ein Konto Azure-Speicher bereitstellt. Wenn Code in einer Rolle ausgeführten Code Azure-Speicher Services Daten – d. h., Blob, Warteschlange oder Tabellendaten – zugreifen muss, müssen Sie eine Verbindungszeichenfolge für dieses Speicherkonto definieren.

Eine Verbindungszeichenfolge, die mit einer Firma Azure-Speicher verweist, muss ein definierten Format verwenden. Informationen zum Erstellen von Verbindungszeichenfolgen, finden Sie unter [Konfigurieren von Azure Speicher Verbindungszeichenfolgen](./storage/storage-configure-connection-string.md).

Wenn Sie zum Testen des Diensts gegen die Dienste Azure-Speicher bereit sind, oder wenn Sie in der Cloud-Dienst in Azure bereitstellen bereit sind, können Sie den Wert der mit Ihrem Konto Azure-Speicher verweisen jede beliebige Verbindungszeichenfolge ändern. Wählen Sie****(...), wählen Sie die **EINGABETASTE Speicher Anmeldeinformationen**. Geben Sie Ihre Kontoinformationen, die Ihren Kontonamen und kontoschlüssel enthält. Klicken Sie im Dialogfeld **Verbindungszeichenfolge für Speicher-Konto** können Sie auch angeben, ob Sie die standardmäßigen HTTPS Endpunkte (die Standardeinstellung), die standardmäßigen HTTP-Endpunkte oder benutzerdefinierten Endpunkte verwenden möchten. Möglicherweise möchten benutzerdefinierte Endpunkte verwenden, wenn Sie einen benutzerdefinierten Domänennamen für Ihren Dienst registriert haben, wie unter [Konfigurieren eines benutzerdefinierten Domänennamens für BLOB-Daten in einem Konto Azure-Speicher](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Ändern Sie Ihre Verbindungszeichenfolgen mit einer Firma Azure-Speicher verweisen, bevor Sie mit den Dienst bereitstellen. Ist dies nicht der Fall kann dazu führen, dass Ihre Rolle nicht zu beginnen, oder zum Umschalten zwischen den Staaten Initialisierung, beschäftigt und beenden.

## <a name="endpoints-page"></a>Endpunkte Seite

Eine Worker-Rolle kann eine beliebige Anzahl von HTTP, HTTPS oder TCP Endpunkte haben. Endpunkte möglich Eingabewerte Endpunkte, die für externe Clients verfügbar sind, oder internen Endpunkte, die anderen Rollen zur Verfügung stehen, die im Dienst ausgeführt werden.

- Um einen HTTP-Endpunkt externe Clients und Webbrowser verfügbar zu machen, ändern Sie den Endpunkttyp zur Eingabe, und geben Sie einen Namen und eine öffentliche Port-Nummer.

- Um einen HTTPS-Endpunkt externe Clients und Webbrowser verfügbar zu machen, ändern Sie der Endpunkt **von**, und geben Sie einen Namen, eine öffentliche Port-Nummer und einer Zertifikatsname Management.

    Beachten Sie, bevor Sie ein Zertifikat Management angeben können, müssen Sie das Zertifikat auf der Seite der Eigenschaft **Zertifikate** definieren.

- Um einen Endpunkt durch andere Rollen in der Cloud-Dienst für den internen Zugriff verfügbar zu machen, ändern Sie den Endpunkttyp in internen, und geben Sie einen Namen und mögliche private Ports für diesen Endpunkt.

## <a name="local-storage-page"></a>Lokaler Speicher-Seite

Die Eigenschaftenseite für **Lokale Speicher** können eine oder mehrere lokale Speicherressourcen für eine Rolle reservieren. Eine lokale Speicherressource ist ein reservierte Verzeichnis im Dateisystem von der Azure-virtuellen Computern eine Instanz von einer Rolle ausgeführt wird.

## <a name="certificates-page"></a>Seite Zertifikate

Klicken Sie auf der Seite **Zertifikate** können Sie Ihre Rolle Zertifikate zuordnen. Die Zertifikate, die Sie hinzufügen können zum Konfigurieren Ihrer Endpunkte HTTPS auf die **Endpunkte** Eigenschaftenseite verwendet werden.

Die **Zertifikate** Eigenschaftenseite hinzugefügt Ihrer Dienstkonfiguration Informationen über Ihre Zertifikate. Beachten Sie, dass Ihre Zertifikate nicht mit Ihrem Dienst verpackt werden. Sie müssen Ihre Zertifikate separat auf Azure über das [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen.

Geben Sie einen Namen für das Zertifikat, um Ihre Rolle ein Zertifikat zuzuordnen. Verwenden Sie diesen Namen zum Verweisen auf das Zertifikat, wenn Sie einen HTTPS-Endpunkt auf der Eigenschaftenseite **Endpunkte** konfigurieren. Geben Sie nun, ob der Zertifikat Store **Lokaler Computer** oder einen **Aktuellen Benutzer** und den Namen des Speichers ist. Geben Sie schließlich Fingerabdruck des Zertifikats. Wenn das Zertifikat in der aktuellen Benutzer\Persönlich (Mein) Store ist, können Sie der Fingerabdruck des Zertifikats, indem Sie das Zertifikat aus einer Liste eingetragenen auswählen eingeben. Wenn sie in einem anderen Speicherort befindet, geben Sie den Fingerabdruckwert von hand.

Wenn Sie ein Zertifikat aus dem Zertifikatspeicher hinzufügen, werden alle zwischen-XT für Zertifikate automatisch an den Einstellungen Konfiguration für Sie hinzugefügt. Zwischen-XT für diese Zertifikate hinzufügen müssen auch in Azure hochgeladen werden, um ordnungsgemäß des Diensts für SSL konfigurieren.

Alle Management Zertifikate, die Sie mit Ihrem Dienst zuordnen anwenden an den Dienst nur, wenn sie in der Cloud ausgeführt wird. Wenn der lokale Entwicklungsumgebung des Diensts ausgeführt wird, wird ein standard-Zertifikat, das von der Serveremulator verwaltet wird verwendet.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfigurieren das Projekt Azure-Cloud-Dienst

Zum Konfigurieren der Einstellungen, die für das Projekt für eine gesamte Azure-Cloud-Dienst gelten, öffnen Sie zuerst im Kontextmenü für diesen Projektknoten, und wählen Sie dann öffnen Sie die entsprechenden Eigenschaftsseiten Eigenschaften. Die folgende Tabelle zeigt die Eigenschaftenseiten.

|Eigenschaftenseite|Beschreibung|
|---|---|
|Anwendung|Auf dieser Seite können Sie anzeigen Informationen über die Version von Azure-Tools, die dieses Projekt Cloud-Dienst verwendet, und Sie können auf die aktuelle Version der Tools aktualisieren.|
|Erstellen von Ereignissen|Auf dieser Seite können Sie vor und nach dem Build Ereignisse festlegen.|
|Entwicklung|Auf dieser Seite können Sie angeben, die Konfiguration Anweisungen erstellen und die Umstände, die unter denen Ereignissen nach dem Build ausgeführt werden.|
|Web|Auf dieser Seite können Sie die Einstellungen konfigurieren, die an den Webserver beziehen.|
