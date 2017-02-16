<properties 
   pageTitle="Erstellen von selbstsignierten Zertifikaten für virtuelle Punkt-zu-Standort-Netzwerk Cross Verbindungen lokale mithilfe von Makecert | Microsoft Azure"
   description="Dieser Artikel enthält Schritte aus, um Makecert zum Erstellen von selbstsignierten Zertifikaten auf Windows-10 verwenden."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Arbeiten mit selbstsignierten Zertifikaten für Punkt-zu-Standort-Verbindungen

In diesem Artikel hilft Ihnen ein selbst signiertes Zertifikat verwenden **Makecert**erstellen, und dann Client-Zertifikate daraus generieren. Die Schritte sind für Makecert auf Windows 10 geschrieben. MakeCert wurde bestätigt, um Zertifikate zu erstellen, die mit P2S Verbindungen kompatibel sind. 

Für Verbindungen P2S die bevorzugte Methode für die Feiertage besteht darin, Ihre Lösung Enterprise Zertifikat, und stellen Sie sicher, den Client mit der gemeinsamen Wert Namensformat Zertifikate auszustellen verwenden 'name@yourdomain.com', und nicht das Format 'Domänenname\Benutzername NetBIOS'.

Wenn Sie eine Enterprise-Lösung verfügen, ist ein selbst signiertes Zertifikat erforderlich, vor P2S Clients für die Verbindung zu einem virtuellen Netzwerk zulassen. Wir sind bewusst, dass Makecert ist veraltet, aber es immer noch eine gültige Methode zum Erstellen von selbstsignierten Zertifikaten, die mit P2S Verbindungen kompatibel sind. Wir arbeiten an einer anderen Lösung zum Erstellen von selbstsignierten Zertifikaten, aber zu diesem Zeitpunkt Makecert ist die bevorzugte Methode.

## <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats

MakeCert ist eine Möglichkeit, ein selbst signiertes Zertifikat erstellen. Die folgenden Schritte aus, die Sie Erstellen eines selbstsignierten Zertifikats mit Makecert durchgehen. Diese Schritte sind nicht-Modell zur Bereitstellung von bestimmten. Sie sind für sowohl Ressourcenmanager und klassischen gültig.

### <a name="to-create-a-self-signed-certificate"></a>Zum Erstellen eines selbstsignierten Zertifikats

1. Herunterladen Sie von einem Computer unter Windows 10 und installieren Sie das [Windows Software Development Kit (SDK) für Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Nach der Installation können Sie das Programm makecert.exe unter dieser Pfad finden: c:\Programme Dateien (x86) \Windows Kits\10\bin\<einem Bogen >. 
        
    Beispiel:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Als Nächstes erstellen und Installieren eines Zertifikats im Speicher persönlichen Zertifikats auf Ihrem Computer. Im folgenden Beispiel wird eine entsprechende *CER* -Datei, die Sie in Azure hochladen, wenn P2S konfigurieren. Den folgenden Befehl als Administrator ausführen. Ersetzen Sie *ARMP2SRootCert* und *ARMP2SRootCert.cer* durch den Namen, den Sie für das Zertifikat verwenden möchten.<br><br>Das Zertifikat wird in Ihrer Zertifikate - Aktueller Benutzer\Persönlich\Zertifikate befinden.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="a-namerootpublickeyato-obtain-the-public-key"></a><a name="rootpublickey"></a>Um den öffentlichen Schlüssel zu erhalten.

Als Teil der VPN-Gateway-Konfiguration für Punkt-zu-Standort Verbindungen wird der öffentliche Schlüssel für das Zertifikat der Stamm in Azure hochgeladen.

1. Um eine CER-Datei aus dem Zertifikat zu erhalten, öffnen Sie **certmgr.msc**ein. Mit der rechten Maustaste in des Zertifikats selbstsignierten Stamm, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**. Daraufhin wird ein **Zertifikat Export-Assistenten**.

2. Im Assistenten klicken Sie auf **Weiter**, wählen Sie **Nein, privaten Schlüssel nicht exportieren**aus, und klicken Sie dann auf **Weiter**.

3. Klicken Sie auf der Seite **Format der zu exportierenden Datei** wählen **Base-64-codierte x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Klicken Sie auf die **Datei zu exportieren**, **Navigieren** Sie zu dem Speicherort, den Sie das Zertifikat exportieren möchten. Dateinamen Sie für den **Dateinamen ein**den Zertifikat aus. Klicken Sie dann auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

 
### <a name="export-the-self-signed-certificate-optional"></a>Exportieren Sie das selbst signierte Zertifikat (optional)

Möglicherweise möchten das selbst signierte Zertifikat exportieren und sicheres speichern. Wenn werden müssen, Sie später auf einem anderen Computer installieren und weitere Client-Zertifikate generieren oder ein anderes CER-Datei exportieren. Jedem Computer mit einer Client-Zertifikat installiert und die ist auch mit dem entsprechenden VPN-Client konfiguriert werden, die Einstellungen auf das virtuelle Netzwerk über P2S eine Verbindung herstellen können. Aus diesem Grund soll um sicherzustellen, dass Client-Zertifikate generiert und nur bei Bedarf installiert sind und das selbst signierte Zertifikat sicher gespeichert sind.

Um das selbst signierte Zertifikat als einer PFX-Datei exportieren, wählen Sie das Zertifikat aus, und verwenden Sie dieselben Schritte zum Exportieren in [ein Client-Zertifikat exportieren](#clientkey) beschriebenen.

## <a name="create-and-install-client-certificates"></a>Erstellen und Installieren von Client-Zertifikate

Sie installieren nicht selbst signierte Zertifikat direkt auf dem Clientcomputer. Sie müssen ein Client-Zertifikat aus dem selbst signiertes Zertifikat generieren. Sie exportieren und installieren Sie das Clientzertifikat auf dem Clientcomputer. Die folgenden Schritte sind nicht-Modell zur Bereitstellung von bestimmten. Sie sind für sowohl Ressourcenmanager und klassischen gültig.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Teil 1 – erstellen Sie ein Clientzertifikat aus ein selbst signiertes Zertifikat

Die folgenden Schritte durchgehen eine Möglichkeit, ein Client-Zertifikat aus ein selbst signiertes Zertifikat generieren. Sie können mehrere Client-Zertifikate aus das gleiche Zertifikat generieren. Jede Client-Zertifikat kann exportiert und auf dem Clientcomputer installiert werden. 

1. Öffnen Sie auf dem gleichen Computer, den Sie zum Erstellen des selbst signierten Zertifikats verwendet ein Eingabeaufforderungsfenster als Administrator.

2. Bezieht sich in diesem Beispiel "ARMP2SRootCert" auf das selbst signiertes Zertifikat, das Sie generiert ein. 
    - Ändern Sie *"ARMP2SRootCert"* auf den Namen des selbstsignierten Stamm, dem Sie aus das Client-Zertifikat generieren. 
    - Ändern Sie *ClientCertificateName* auf den Namen, den ein Client-Zertifikats benutzerspezifisch generiert werden sollen. 


    Ändern Sie, und führen Sie das Beispiel, um ein Client-Zertifikat generieren. Wenn Sie das folgende Beispiel ausführen, ohne Sie zu ändern, ist das Ergebnis ein Client-Zertifikat, das mit dem Namen ClientCertificateName in Ihrer persönlichen Zertifikats Store, die aus dem Stammordner Zertifikat ARMP2SRootCert generiert wurde.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Alle Zertifikate in Ihre "Zertifikate - Aktueller Benutzer\Persönlich\Zertifikate' gespeichert werden auf Ihrem Computer speichern. Sie können generieren, wie viele Client-Zertifikate nach Bedarf zu diesem Verfahren basiert.

### <a name="a-nameclientkeyapart-2---export-a-client-certificate"></a><a name="clientkey"></a>Teil 2: Exportieren eines Client-Zertifikats

1. Wenn ein Client-Zertifikat exportieren möchten, öffnen Sie **certmgr.msc**ein. Mit der rechten Maustaste in des Client-Zertifikats, das Sie exportieren, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**möchten. Daraufhin wird ein **Zertifikat Export-Assistenten**.

2. Klicken Sie im Assistenten klicken Sie auf **Weiter**, und klicken Sie dann wählen Sie **Ja, privaten Schlüssel exportieren**aus, und klicken Sie dann auf **Weiter**.

3. Klicken Sie auf der Seite **Dateiformat exportieren** können Sie die Standwerte lassen. Klicken Sie dann auf **Weiter**. 
 
4. Klicken Sie auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie mit einem Kennwort auswählen, stellen Sie sicher, aufzeichnen oder das Kennwort ein, die Sie für dieses Zertifikat festlegen. Klicken Sie dann auf **Weiter**.

5. Klicken Sie auf die **Datei zu exportieren**, **Navigieren** Sie zu dem Speicherort, den Sie das Zertifikat exportieren möchten. Dateinamen Sie für den **Dateinamen ein**den Zertifikat aus. Klicken Sie dann auf **Weiter**.

6. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.  

### <a name="part-3---install-a-client-certificate"></a>Teil 3: Installieren eines Clientzertifikats

Jeder Client, auf die gewünschte Verbindung mit Ihrem Netzwerk virtuelle mithilfe einer Punkt-zu-Standort-Verbindungs müssen ein Client-Zertifikat installiert. Dieses Zertifikat ist zusätzlich zu den erforderlichen VPN-Konfigurationspaket. Die folgenden Schritte aus, die Sie Manuelles Installieren von dem Client-Zertifikat durchgehen.

1. Suchen Sie und kopieren Sie die *PFX* -Datei auf dem Clientcomputer. Doppelklicken Sie auf die *PFX* -Datei zu installieren, auf dem Clientcomputer. Lassen Sie den **Speicherort** als **Aktueller Benutzer**, und klicken Sie auf **Weiter**.

2. Klicken Sie auf die **Datei** auf der Seite zu importieren ändern Sie nicht. Klicken Sie auf **Weiter**.

3. Geben Sie auf der Seite **Schutz privater Schlüssel** das Kennwort für das Zertifikat verwendet eine, oder stellen Sie sicher, dass die Sicherheit der Tilgungsanteile, die das Zertifikat installiert werden korrekt ist, und klicken Sie auf **Weiter**.

4. Klicken Sie auf der Seite **Zertifikat Store** lassen Sie den Standardspeicherort, und klicken Sie dann auf **Weiter**.

5. Klicken Sie auf **Fertig stellen**. **Sicherheitshinweis** für das Zertifikat installieren klicken Sie auf **Ja**. Das Zertifikat wurde erfolgreich importiert.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit der Konfiguration Punkt-zu-Standort. 

- **Ressourcenmanager** Bereitstellung Modell Anweisungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung mit einer VNet mithilfe der PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klassische** Bereitstellung Modell Anweisungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort VPN-Verbindung zu einer VNet über das klassische-Portal](vpn-gateway-point-to-site-create.md).