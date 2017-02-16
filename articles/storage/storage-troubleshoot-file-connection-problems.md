<properties
    pageTitle="Behandeln von Problemen mit Speicher Azure Datei | Microsoft Azure"
    description="Behandeln von Problemen mit Speicher Azure-Datei"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Problembehandlung bei der Datei Azure-Speicher

Dieser Artikel enthält häufig auftretende Probleme, die mit Microsoft Azure Dateispeicher zusammenhängen, wenn Sie von Windows und Linux-Clients verbinden. Darüber hinaus den möglichen Ursachen und Lösungen für diese Probleme.

**Allgemeine Probleme (in Windows und Linux-Clients auftreten)**

- [Speicherkontingent zurück, bei dem Versuch, eine Datei zu öffnen](#quotaerror)

- [Beim Zugriff auf Azure Dateispeicher von Windows oder Linux langsam](#slowboth)

**Windows-Client-Problemen**

- [Geringe Leistung, wenn Sie Windows 8.1 oder Windows Server 2012 R2 Azure Dateispeicher zugreifen](#windowsslow)

- [Fehler beim Versuch, eine Dateifreigabe Azure bereitzustellen 53](#error53)

- [Netto verwenden war erfolgreich, aber die Azure-Datei in Windows-Explorer bereitgestellten freigeben wird nicht angezeigt](#netuse)

- [Mein Speicherkonto enthält "/" und verwenden Sie das Netz Befehl fehlschlägt](#slashfails)

- [Meine Anwendungsdienst zugreifen nicht Laufwerk Azure-Dateien.](#accessfiledrive)

- [Zusätzliche Informationen, um die Leistung zu optimieren.](#additional)

**Linux Client Probleme**

- ["Sie sind kopieren eine Datei in ein Ziel, die keine Verschlüsselung unterstützt" Fehler beim Hochladen/Kopieren von Dateien in Azure-Dateien](#encryption)

- [Fehler "Host ist nach unten", klicken Sie auf vorhandene Datei freigegeben, oder die Verwaltungsshell hängt beim Ausführen der Liste Befehle auf den Bereitstellungspunkt](#errorhold)

- [Bereitstellen Sie Fehler 115, wenn Sie auf bereitstellen Azure-Dateien auf die Linux VM versuchen](#error15)

- [Linux VM aufgetreten random verzögert Befehle wie "ls"](#delayproblem)

## <a name="general-problems"></a>Allgemeine Probleme
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Speicherkontingent zurück, bei dem Versuch, eine Datei zu öffnen

Unter Windows erhalten Sie Fehlermeldungen, die wie folgt aussehen:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Nicht genügend Kontingent steht für diesen Befehl.**

**Ungültiges Handlewert GetLastError: 53**

Klicken Sie auf Linux erhalten Sie Fehlermeldungen, die wie folgt aussehen:

**<filename>[Zugriff verweigert]**

**Datenträgerkontingent überschritten**

#### <a name="cause"></a>Ursache

Das Problem tritt auf, da Sie die Obergrenze des gleichzeitigen Öffnen Ziehpunkte erreicht haben, die für eine Datei zulässig ist.

#### <a name="solution"></a>Lösung

Verringern Sie die Anzahl der gleichzeitigen Öffnen Ziehpunkte, indem einige Ziehpunkte schließen, und führen Sie dann erneut. Weitere Informationen finden Sie unter [Microsoft Azure-Speicher Leistung und Skalierbarkeit Checkliste](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Beim Speichern von Daten von Windows oder Linux Zugriff auf langsam

- Wenn Sie eine bestimmte e/a-Größe Mindestanforderungen besitzen, empfehlen wir, dass Sie 1 MB als die e/a-Größe für optimale Leistung verwenden.

- Wenn Sie wissen, dass die endgültige Größe einer Datei, die Sie mit schreibt erweitert werden und die Software nicht Kompatibilitätsprobleme, wenn das Ende des noch nicht geschrieben haben, klicken Sie auf die Datei mit Nullen, legen Sie die Dateigröße, statt jede schreiben im Voraus eine Erweitern von schreiben wird.

## <a name="windows-client-problems"></a>Windows-Client-Problemen
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Wenn die Speicherung von Dateien aus Windows 8.1 oder Windows Server 2012 R2 den Zugriff auf langsam

Stellen Sie für Clients, die Windows 8.1 oder Windows Server 2012 R2 arbeiten sicher, dass das Update [KB3114025](https://support.microsoft.com/kb/3114025) installiert ist. Dieses Update verbessert die erstellen und Leistung Handle zu schließen.

Sie können das folgende Skript, um zu überprüfen, ob das Update auf installiert wurde, ausführen:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Wenn Update installiert ist, wird die folgende Ausgabe angezeigt:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0 x 1**

> [AZURE.NOTE]  Windows Server 2012 R2 Bilder in Azure Marketplace müssen das Update KB3114025 standardmäßig starten im Dezember 2015 installiert.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Zusätzliche Informationen, um die Leistung zu optimieren.

Nie erstellen Sie oder öffnen Sie eine Datei für zwischengespeicherte e/a, der Schreibzugriff aber nicht Lesezugriff anfordert. Das heißt, wenn Sie **CreateFile()**anrufen, nie nur **GENERIC_WRITE**, aber immer angeben **GENERIC_READ | GENERIC_WRITE**. Nur Schreiben Griff kann nicht kleine schreibt lokal, auch wenn es der einzige geöffnete Handle für die Datei ist Zwischenspeichern. Klicken Sie auf kleine schreibt bedeutet dies eine schwerwiegende Leistung beeinträchtigt. Beachten Sie, dass der "a" Modus CRT **fopen()** nur schreiben Griff wird geöffnet.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Fehler 53" beim Versuch, aktivieren oder Deaktivieren einer Dateifreigabe Azure

Dieses Problem kann folgende Ursachen haben:

#### <a name="cause-1"></a>Ursache 1

"Systemfehler 53 ist aufgetreten. Zugriff verweigert." Aus Gründen der Sicherheit sind Verbindungen zu Freigaben Azure-Dateien blockiert, wenn der Kommunikationschannel ist nicht verschlüsselt und der Verbindungsversuch besteht nicht aus dem gleichen Data Center auf dem Azure Dateifreigaben befinden. Kommunikation Channelverschlüsselung ist nicht verfügbar, wenn der Client des Benutzers OS SMB-Verschlüsselung unterstützt. Dies wird angegeben, indem eine "Systemfehler 53 ist aufgetreten. Zugriff verweigert"Fehlermeldung, wenn ein Benutzer versucht, eine Dateifreigabe aus lokalen oder von einem anderen Data Center bereitzustellen. Windows 8, Windows Server 2012 und höhere Versionen jeder Negotiate Anforderung, die SMB 3.0, enthält die Verschlüsselung unterstützt.

#### <a name="solution-for-cause-1"></a>Lösung für Ursache 1

Verbinden von einem Client, die die Anforderungen der Windows 8, Windows Server 2012 oder höhere Versionen entspricht oder Besprechung herstellen möchten, die aus einem virtuellen Computern, die auf das gleiche Data Center als Speicher Azure-Konto handelt, verwendet wird, für die Dateifreigabe Azure.

#### <a name="cause-2"></a>Ursache 2

"Systemfehler 53" Wenn Sie bereitstellen kann eine Dateifreigabe Azure auftreten, wenn Port 445 ausgehende Kommunikation auf Azure Dateien Datacenter blockiert wird. Klicken Sie auf [hier](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) Zusammenfassung der Internetdienstanbieter angezeigt, die erlauben oder verbieten von Access aus Port 445.

Comcast und einige IT-Organisationen blockieren diesen Anschluss. Um zu verstehen, ob dies der Grund für die Meldung "Systemfehler 53" ist, können Sie Portqry den Endpunkt TCP:445 Abfragen. Wenn Sie der Endpunkt TCP:445 als gefiltert angezeigt wird, ist die TCP-blockiert. Hier ist eine Beispiel für eine Abfrage:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Wenn die TCP 445 mithilfe einer Regel entlang des Pfads Netzwerk blockiert werden, wird die folgende Ausgabe angezeigt:

**TCP-Port 445 (Microsoft-ds-Dienst): gefiltert**

Weitere Informationen zur Verwendung von Portqry finden Sie unter [Beschreibung des Befehlszeilenprogramms Portqry.exe](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Lösung für Ursache 2

Arbeiten Sie mit Ihrer IT-Abteilung Port 445 ausgehende [Azure IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653)zu öffnen.

#### <a name="cause-3"></a>Ursache 3

"Systemfehler 53" kann auch empfangen werden, wenn NTLMv1 Kommunikation auf dem Client aktiviert ist. NTLMv1 aktiviert haben, wird einen weniger sichere Client erstellt. Daher werden für Dateien Azure Kommunikation blockiert. Zum Überprüfen, ob dies die Ursache des Fehlers ist, vergewissern Sie sich, dass der folgenden Unterschlüssel der Registrierung auf einen Wert von 3 festgelegt ist:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Weitere Informationen finden Sie unter dem Thema [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) auf TechNet.

#### <a name="solution-for-cause-3"></a>Lösung für Ursache 3

Um dieses Problem zu beheben, Zurücksetzen Sie LmCompatibilityLevel im Registrierungsschlüssel HKLM\SYSTEM\CurrentControlSet\Control\Lsa auf den Standardwert von 3.

Azure-Dateien unterstützt nur NTLMv2-Authentifizierung. Stellen Sie sicher, dass Gruppenrichtlinien auf die Clients angewendet wird. Dies wird dieser Fehler zu vermeiden. Dies gilt auch aus Sicherheitsgründen werden. Weitere Informationen finden Sie unter [Clients zur Verwendung von NTLMv2 konfigurieren mithilfe von Gruppenrichtlinien](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Die richtlinieneinstellung empfohlene ist **nur NTLMv2-Antworten**. Dies entspricht einem Registrierungseintrag von 3. Clients verwenden nur NTLMv2-Authentifizierung, und verwenden sie NTLMv2 Sitzung Sicherheit, wenn der Server unterstützt. Domänencontroller akzeptieren LM, NTLM und NTLMv2-Authentifizierung.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Netto verwenden war erfolgreich, aber die Azure-Datei in Windows-Explorer bereitgestellten freigeben wird nicht angezeigt

#### <a name="cause"></a>Ursache

Standardmäßig wird die Windows-Explorer nicht als Administrator ausgeführt. Wenn Sie über die Befehlszeile ein Administrator **Netto verwenden** ausführen, ordnen Sie das Netzlaufwerk "als"Administrator. Da zugeordnete Laufwerke Benutzer reduzierte haben, werden das Benutzerkonto, das angemeldet ist keine Laufwerke angezeigt, wenn diese unter einem anderen Benutzerkonto bereitgestellt werden.

#### <a name="solution"></a>Lösung

Stellen Sie die Freigabe über die Befehlszeile nicht Administrator bereit. Alternativ können Sie [diesem TechNet-Artikel](https://technet.microsoft.com/library/ee844140.aspx) , um den Registrierungseintrag **EnableLinkedConnections** konfigurieren:

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Mein Speicherkonto enthält "/" und verwenden Sie das Netz Befehl fehlschlägt

#### <a name="cause"></a>Ursache

Wenn Sie der Befehl **Netto Use** unter Eingabeaufforderungsfenster (cmd.exe) ausgeführt wird, wird sie analysiert durch Hinzufügen von "/" als Befehlszeilenoption. Dadurch wird die Zuordnung zum Fehlschlagen des Laufwerks.

#### <a name="solution"></a>Lösung

Verwenden Sie eines der folgenden Schritte aus, um das Problem zu beheben:

• Verwenden Sie den folgenden PowerShell-Befehl:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Aus einer Batchdatei kann dies als vorgenommen werden

`Echo new-smbMapping ... | powershell -command –`

• Setzen Sie die EINGABETASTE, um dieses Problem zu umgehen doppelten Anführungszeichen ein – es sei denn, "/" ist das erste Zeichen. Wenn Ja, verwenden Sie den interaktiven Modus und geben Sie Ihr Kennwort separat oder neu generieren Sie der Schlüssel, um einen Schlüssel zu erhalten, der nicht mit dem Schrägstrich (/) beginnt.

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Meine Anwendungsdienst kann nicht bereitgestelltes Dateien Azure-Laufwerk zugegriffen werden.

#### <a name="cause"></a>Ursache

Laufwerke werden pro Benutzer bereitgestellt. Wenn die Anwendung oder den Dienst unter einem anderen Benutzerkonto ausgeführt wird, werden das Laufwerk Benutzer nicht angezeigt.

#### <a name="solution"></a>Lösung

Stellen Sie Laufwerk aus dem gleichen Benutzerkonto, unter dem die Anwendung ist. Dies kann mithilfe von Tools wie Psexec.

Alternativ können Sie einen neuen Benutzer, der weist dieselben Berechtigungen wie die Netzwerkkonto Dienst oder System erstellen und führen Sie anschließend **Cmdkey** und **Netto verwenden** , klicken Sie unter diesem Konto. Der Benutzername den Kontonamen Speicher sein sollte, und Kennwort sollten die Speicher Konto-Taste. Eine andere Option für die **Verwendung der Netto** ist im Speicher Kontonamen und Schlüssel in der Parameter für und das Kennwort des Befehls **Netto verwenden** übergeben.

Nachdem Sie diese Schritte befolgen, möglicherweise die folgende Fehlermeldung: "Systemfehler 1312 aufgetreten. Eine angegebenen Sitzung ist nicht vorhanden. Es möglicherweise bereits wurden, beendet" **Netto verwenden** des Systems/Network-Dienstkontos beim Ausführen. Wenn diese Situation vorliegt, stellen Sie sicher, dass der Benutzername, der **Netto** verwenden übergeben wird Domäneninformationen enthält (Beispiel: "[Speicher Kontonamen]. file.core.windows .net").

## <a name="linux-client-problems"></a>Linux Client Probleme

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fehlermeldung "Sie eine Datei in einem Ziel, das keine Verschlüsselung unterstützt kopieren"

#### <a name="cause"></a>Ursache

BitLocker verschlüsselt Dateien können in Azure Dateien kopiert werden. Der Dateispeicher unterstützt NTFS EFS jedoch nicht. Sie sind daher wahrscheinlich EFS in diesem Fall verwenden. Sie Dateien haben, die durch EFS verschlüsselt sind, kann ein Kopiervorgang auf den Datei-Speicher fehl, wenn des Befehls kopieren eine kopierte Datei entschlüsseln ist.

#### <a name="workaround"></a>Dieses Problem zu umgehen

Um eine Datei in die Speicherung von Dateien zu kopieren, müssen Sie es zuerst entschlüsseln. Sie können dazu eine der folgenden Methoden verwenden:

• Verwenden Sie **Kopieren/d**.

• Legen Sie den folgenden Registrierungsschlüssel:

- Pfad = HKLM\Software\Policies\Microsoft\Windows\System
- Werttyp = DWORD
- Name = CopyFileAllowDecryptedRemoteDestination
- Wert = 1

Beachten Sie aber, dass Festlegen des Registrierungsschlüssels wirkt sich auf alle Kopiervorgänge Netzwerk-Freigaben.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Fehler "Host ist nach unten", klicken Sie auf vorhandene Datei freigegeben, oder die Verwaltungsshell hängt, wenn Sie Befehle auflisten auf den Bereitstellungspunkt ausführen

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf dem Linux-Client, bei der Client für eine längere Zeit im Leerlauf war. Wenn dieser Fehler auftritt, der Client getrennt wird und der Clientverbindung erreicht ist.

#### <a name="solution"></a>Lösung

Dieses Problem ist jetzt im Linux Kernel als Teil der [Menge ändern](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), steht noch aus Backport in Linux Verteilung behoben.

Zu funktioniert, um dieses Problem zu umgehen die Verbindung aufrechterhalten und zu verhindern, dass in Leerlauf, belassen einer Datei in die Azure Dateifreigabe, der Sie regelmäßig zu schreiben. Dies hat einen Schreibvorgang, z. B.-neuschreibung das Datum das erstellt/Änderung an der Datei sein. Andernfalls möglicherweise zwischengespeicherten Ergebnisse erhalten, und der Vorgang möglicherweise nicht die Verbindung auslösen.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Fehler 115 bereitgestellt" beim Versuch, Azure-Dateien auf die Linux VM bereitzustellen.

#### <a name="cause"></a>Ursache

Linux-Versionen unterstützt nicht noch Verschlüsselungsfeature in SMB 3.0. In einigen Verteilung möglicherweise Benutzer eine "115" Fehlermeldung, wenn Benutzer bereitstellen Azure Dateien versuchen, mithilfe von SMB 3.0 aufgrund eines fehlenden Features.

#### <a name="solution"></a>Lösung

Wenn der Linux SMB-Client, der verwendet wird keine Verschlüsselung unterstützt, Dateien bereitstellen Azure mithilfe von SMB 2.1 aus einer Linux VM auf das gleiche Data Center als Datei Speicher-Konto.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux VM aufgetreten random verzögert Befehle wie "ls"

#### <a name="cause"></a>Ursache

Dies kann geschehen, wenn der Befehl ' bereitstellen ' die Option **Serverino** nicht enthalten ist. Ohne **Serverino**führt der Befehl ls einer **Statistik** auf jede Datei.

#### <a name="solution"></a>Lösung

Überprüfen Sie die **Serverino** in Ihrem Eintrag "/ usw./Fstab":

azureuser.File.Core.Windows.NET/WMS/comer auf/Start/Sampledir Typ Cifs (Rw, Nodev, Relatime, Vers. = 2.1, sec = Ntlmssp, Cache = strict, Username = Funktionen Länge und LÄNGEB, Domäne = X, File_mode = 0755, Dir_mode = 0755, Serverino, Rsize = 65536, "wsize" = 65536, Actimeo = 1)

Wenn die Option **Serverino** nicht vorhanden ist, heben Sie die Bereitstellung und Azure-Dateien erneut bereitstellen, indem Sie die Option **Serverino** ausgewählt.

## <a name="learn-more"></a>Weitere Informationen

- [Erste Schritte mit auf Windows Azure-Datei-Speicher](storage-dotnet-how-to-use-files.md)

- [Erste Schritte mit Azure Dateispeicher auf Linux](storage-how-to-use-files-linux.md)
