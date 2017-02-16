<properties
    pageTitle="So generieren und übertragen HSM geschützte Schlüssel für Azure-Taste Tresor | Microsoft Azure"
    description="Lesen Sie diesen Artikel, mit deren Hilfe Sie planen, generieren und dann eigene HSM geschützte Schlüssel zur Verwendung mit Azure-Taste Tresor übertragen."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>So generieren und übertragen HSM geschützt Tastenkombinationen für die Taste Tresor Azure

##<a name="introduction"></a>Einführung

Hinzugefügten Assurance bei der Verwendung von Azure-Taste Tresor, können Sie importieren oder generieren Tasten in Hardware Security Module (HSMs), die die Begrenzungslinie HSM nie lassen. Dieses Szenario wird häufig als *bringen Sie Ihren eigenen Schlüssel*oder BYOK bezeichnet. Die HSMs sind FIPS 140-2 Ebene 2 überprüft. Azure-Taste Tresor verwendet Thales nShield Familie HSMs, um Ihre Schlüssel schützen.

Verwenden Sie die Informationen in diesem Thema, mit deren Hilfe Sie planen, generieren und dann eigene HSM geschützte Schlüssel zur Verwendung mit Azure-Taste Tresor übertragen.

Dieses Feature ist nicht verfügbar für Azure China. 

>[AZURE.NOTE] Weitere Informationen zur Azure-Taste Tresor, finden Sie unter [Neuigkeiten Azure-Taste Tresor?](key-vault-whatis.md)  
>
>Ein beim Abrufen Schritte, wozu auch Erstellen einer Key Tresor HSM geschützt werden Tastenkombinationen, finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](key-vault-get-started.md).

Weitere Informationen zu generieren und Übertragung einer Taste HSM geschützt über das Internet:

- Sie generieren die Taste aus einer offline Arbeitsstationen, wodurch die Oberfläche Angriffen reduziert wird.

- Der Schlüssel wird mit einer Taste Exchange Key (KEK) verschlüsselt die verschlüsselte bleibt, bis sie den Azure-Taste Tresor erläutert übertragen werden. Nur die verschlüsselte Version Ihres Schlüssels bewirkt, dass die ursprüngliche Arbeitsstationen.

- Die Extras legt die Eigenschaften für Ihren Mandanten Schlüssel, der in die Welt der Azure-Taste Tresor Sicherheit Ihrer Schlüssel binden. Damit nach der Azure-Taste Tresor HSMs erhalten und Ihre Schlüssel entschlüsseln, können nur diese HSMs. Der Schlüssel kann nicht exportiert werden. Diese Bindung wird durch die Thales HSMs erzwungen.

- Verwendet wird, um den Key verschlüsseln Key Exchange Key (KEK) innerhalb der Azure-Taste Tresor HSMs generiert und kann nicht exportiert werden. Die HSMs erzwingen, dass es kann keine Version der KEK außerhalb der HSMs löschen. Darüber hinaus enthält die Extras Bescheinigung Thales, dass die KEK kann nicht exportiert werden, und innerhalb einer Originalsoftware HSM, die von Thales hergestellt wurde generiert wurde.

- Die Extras enthält Bescheinigung Thales, dass die Azure-Taste Tresor Security World auch auf eine Originalsoftware HSM von Thales hergestellt generiert wurde. Diese Bescheinigung erweist sich als Sie, dass Microsoft Originalsoftware Hardware verwendet wird.

- Microsoft verwendet separate KEKs und Wertpapiers Welt in jeder geografische Region zu trennen. Diese Trennung wird sichergestellt, dass der Schlüssel nur in Data Center in der Region verwendet werden kann, in dem Sie verschlüsselt. Beispielsweise kann ein Schlüssel aus einem europäischen Kunden in Data Center in Nordamerika oder Asien verwendet werden.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Weitere Informationen zu Thales HSMs und Microsoft-Diensten

Thales e-Security ist eine führende Anbieter von Daten für Verschlüsselung und im Internet Sicherheit Lösungen für die Finanzdienstleister, High Tech, Fertigung, Government und IT-Branche. Mit einem 40 Jahren nachverfolgen Eintrag zum Schutz Ihres Unternehmens und Government Informationen werden Thales Lösungen von vier der fünf größten Aufwand und Luftfahrt verwendet. Die dazugehörigen Lösungen von 22 NATO Ländern auch verwendet werden, und Sichern von mehr als 80 Prozent der weltweit Zahlungstransaktionen.

Microsoft hat gemeinsam im Zusammenhang mit Thales den Status der Art für HSMs zu verbessern. Diese Erweiterungen ermöglichen Ihnen typische Vorteile der gehosteten Dienste ohne Freigabe beeinflussen Ihre Keys abrufen. Lassen Sie folgenden Erweiterungen insbesondere Microsoft die HSMs verwalten, damit Sie nicht zu verfügen. Als Clouddienst wird skaliert Azure-Taste Tresor von kurzfristig in Ihrer Organisation Verwendung Spitzen entsprechen. Zur gleichen Zeit, der Key innerhalb des Microsoft HSMs geschützt ist: behalten Sie Kontrolle über die wichtigsten Lebenszyklus, da Sie die Taste generieren und diese von Microsoft erläutert übertragen.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Bringen Sie Ihren eigenen Schlüssel (BYOK) implementieren für Azure-Taste Tresor

Verwenden Sie die folgenden Informationen und Verfahren ein, wenn Sie einen eigenen Schlüssel HSM geschützt generiert und klicken Sie dann auf Azure-Taste Tresor übertragen – den Ihr eigenes Szenario Schlüssel (BYOK).


##<a name="prerequisites-for-byok"></a>Erforderliche Komponenten für BYOK

Finden Sie in der folgenden Tabelle eine Liste der erforderlichen Komponenten für einen eigenen Schlüssel (BYOK) für Azure-Taste Tresor bringen.

|Anforderung|Weitere Informationen|
|---|---|
|Ein Azure-Abonnement|Zum Erstellen einer Azure-Taste Tresor benötigen Sie ein Azure-Abonnement: [Anmeldung für kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)|
|Der Azure-Taste Tresor Premium Service Ebene zur Unterstützung von Tasten HSM geschützt|Weitere Informationen zu den Dienst Ebenen und Funktionen für Azure-Taste Tresor finden Sie unter der Website [Azure Schlüssel Tresor Preise](https://azure.microsoft.com/pricing/details/key-vault/) .|
|Thales HSM, Smartcards und Support-software|Sie müssen Zugriff auf eine Thales Hardware Security Module und grundlegende Kenntnisse Thales HSMs funktionsfähig. Finden Sie unter [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) , für die Liste der kompatiblen Modelle oder eine HSM erwerben, wenn Sie nicht vorhanden ist.|
|Die folgenden Hard- und Software:<ol><li>Eine offline X64 Arbeitsstationen mit einer minimalen Windows-Betriebssystem von Windows 7 und Thales nShield-Software, die mindestens ist Version 11,50.<br/><br/>Wenn diese Arbeitsstationen Windows 7 ausgeführt wird, müssen Sie den [Microsoft .NET Framework 4.5 installieren](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Eine Arbeitsstationen, die mit dem Internet verbunden ist, und hat einen minimalen Windows-Betriebssystem von Windows 7.</li><li>Ein USB-Laufwerk oder ein anderes tragbares Speichergerät, das mindestens 16 MB freier Speicherplatz vorhanden ist.</li></ol>|Aus Sicherheitsgründen empfehlen wir, dass die erste Arbeitsstationen nicht mit einem Netzwerk verbunden ist. Diese Empfehlungen wird jedoch nicht programmgesteuert erzwungen.<br/><br/>Beachten Sie, dass in die folgende Anleitung, diese Arbeitsstationen als die getrennt Arbeitsstationen bezeichnet wird.</p></blockquote><br/>Darüber hinaus ist Ihren Mandanten Schlüssel für ein Netzwerk Herstellung, empfohlen, dass Sie eine zweite, separate Arbeitsstationen verwenden, um die Extras herunterladen und die Taste Mandanten hochladen. Zu Testzwecken, Sie können jedoch über die gleichen Arbeitsstationen als der ersten Phase.<br/><br/>Beachten Sie, dass in die folgende Anleitung, diese second Arbeitsstationen als das Internet verbundenen Arbeitsstationen bezeichnet wird.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generieren und den Key in Azure-Taste Tresor HSM übertragen

Sie verwenden die folgenden fünf Schritte zum Generieren und übertragen Sie Ihre Schlüssel zu einer Azure-Taste Tresor HSM:

- [Schritt 1: Vorbereiten der Internet verbundenen Arbeitsstationen](#step-1-prepare-your-internet-connected-workstation)
- [Schritt 2: Vorbereiten der getrennt Arbeitsstationen](#step-2-prepare-your-disconnected-workstation)
- [Schritt 3: Erstellen des Schlüssels](#step-3-generate-your-key)
- [Schritt 4: Vorbereiten des Schlüssels für die Übertragung](#step-4-prepare-your-key-for-transfer)
- [Schritt 5: Übertragen Sie Ihre Schlüssel zum Azure-Taste Tresor](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Schritt 1: Vorbereiten der Internet verbundenen Arbeitsstationen
Führen Sie die folgenden Verfahren auf Ihrem Computer, die mit dem Internet verbunden ist, für den ersten Schritt.


###<a name="step-11-install-azure-powershell"></a>Schritt 1.1: Installieren Sie Azure PowerShell

Aus dem Internet verbundenen Arbeitsstationen herunterladen und Installieren des Azure-PowerShell-Moduls, das die Cmdlets zum Verwalten von Azure-Taste Tresor enthält. Dies ist die Mindestversion von 0.8.13 erforderlich.

Installation Anweisungen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Schritt 1.2: Rufen Sie Ihrer Azure-Abonnement-ID ab

Starten einer Sitzung Azure PowerShell, und melden Sie sich bei Ihrem Konto Azure mit den folgenden Befehl aus:

        Add-AzureAccount
Geben Sie im Popupmenü Browserfenster Ihren Azure-Konto-Benutzernamen und Ihr Kennwort ein. Verwenden Sie den Befehl [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Suchen Sie anhand der Ausgabe die ID für das Abonnement, für die Taste Tresor Azure erfolgen soll. Sie benötigen diese Abonnement-ID später.

Schließen Sie das Fenster Azure PowerShell nicht.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Schritt 1.3: Die BYOK Extras für Azure-Taste Tresor herunterladen

Wechseln Sie zu der Microsoft Download Center, und [Laden Sie die Azure Schlüssel Tresor BYOK Extras](http://www.microsoft.com/download/details.aspx?id=45345) für Ihre geografische Region oder Instanz von Azure. Verwenden Sie die folgende Informationen, um den Paketnamen herunterladen und deren entsprechenden SHA-256 Paket Hash zu ermitteln:

---

**Nordamerika:**

KeyVault-BYOK-Tools – Vereinigtes States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Europa:**

KeyVault-BYOK-Tools – Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Asien:**

KeyVault-BYOK-Tools – AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Lateinamerika:**

KeyVault-BYOK-Tools – LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japan:**

KeyVault-BYOK-Tools – Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australien:**

KeyVault-BYOK-Tools – Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Tools – USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault-BYOK-Tools – Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Deutschland:**

KeyVault-BYOK-Tools – Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Indien:**

KeyVault-BYOK-Tools – India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Verwenden Sie das Cmdlet " [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) ", um die Integrität der heruntergeladenen BYOK Toolsets aus Ihrer Azure PowerShell-Sitzung zu überprüfen.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Die Extras umfassen Folgendes:

- Ein Key Exchange Key (KEK)-Paket, das mit eine beginnt weist **BYOK-KEK - Pkg-.**
- Ein Security World-Paket, das mit eine beginnt weist **BYOK-SecurityWorld - Pkg-.**
- Ein Python-Skript mit dem Namen V**erifykeypackage.py.**
- Eine Befehlszeile ausführbare Datei benannten **KeyTransferRemote.exe** und zugeordneten DLL-Dateien.
- Ein Visual C++ Redistributable Package, mit dem Namen **vcredist_x64.exe.**

Kopieren Sie das Paket auf einem USB-Laufwerk oder anderen tragbaren Speicher ein.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Schritt 2: Vorbereiten der getrennt Arbeitsstationen

Führen Sie für diesen zweiten Schritt die folgenden Verfahren auf dem Computer, die nicht mit einem Netzwerk (im Internet oder Ihrem internen Netzwerk) verbunden ist.


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Schritt 2.1: Vorbereiten der getrennt Arbeitsstationen mit Thales HSM

Installieren Sie die nCipher (Thales) Support-Software auf einem Windows-Computer, und fügen Sie eine HSM Thales auf diesem Computer.

Stellen Sie sicher, dass die Thales Tools in Ihrem Pfad (**%nfast_home%\bin** und **%nfast_home%\python\bin**) sind. Geben Sie beispielsweise Folgendes ein:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Weitere Informationen finden Sie im Lieferumfang der Thales HSM-Benutzerhandbuch.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Schritt 2.2: Installieren Sie die BYOK Extras auf dem Computer getrennt

Kopieren Sie das BYOK Toolset Paket aus das USB-Laufwerk oder anderen tragbaren Speicher, und klicken Sie dann gehen Sie folgendermaßen vor:

1. Extrahieren Sie die Dateien aus dem heruntergeladenen Paket in einem beliebigen Ordner.
2. Aus diesem Ordner vcredist_x64.exe ausführen.
3. Folgen Sie den Anweisungen in der Installation die Visual C++ Runtime-Komponenten für Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Schritt 3: Erstellen des Schlüssels

Führen Sie für diese dritte Schritt die folgenden Verfahren auf dem Computer getrennt ein.

###<a name="step-31-create-a-security-world"></a>Schritt 3.1: Erstellen einer Wertpapiers Welt

Starten Sie eine Befehlszeile und neue Welt Thales ausgeführt.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Dieses Programm erstellt eine **Security World** -Datei am % NFAST_KMDATA%\local\world, die in den Ordner C:\ProgramData\nCipher\Key Management Data\local entspricht. Sie können verschiedene Werte für das Quorum, aber in diesem Beispiel, werden Sie aufgefordert drei leeren Karten und Stifte für jeden eingeben. Klicken Sie dann gewähren alle zwei Karten Vollzugriff auf die Security World. Diese Karten werden der **Administrator Card Set** für die neue Security World.

Klicken Sie dann folgendermaßen Sie vor:

- Sichern der Weltdatei ein. Secure und schützen Sie die Weltdatei, Administrator Karten und deren Stifte, und stellen Sie sicher, dass keine Einzelperson auf mehr als eine Karte zugreifen kann.

###<a name="step-32-validate-the-downloaded-package"></a>Schritt 3,2: Überprüfen des heruntergeladenen Pakets

Dieser Schritt ist optional, aber empfohlen, damit Sie die folgenden überprüfen können:

- Der Schlüssel Exchange, der in die Extras enthalten ist, wurde aus einer Originalsoftware Thales HSM generiert.
- Der Hash der Welt Sicherheit, die in die Extras enthalten ist, wurde in einer Originalsoftware Thales HSM generiert.
- Der Exchange Schlüssel kann nicht exportiert werden.

>[AZURE.NOTE]Um die heruntergeladene Paket zu überprüfen, das HSM muss verbunden sein, eingeschaltet, und muss eine Wertpapiers Welt daran (beispielsweise das Element, das Sie soeben erstellt haben).

Das heruntergeladene Paket überprüft werden soll:

1.  Führen Sie das Skript verifykeypackage.py, indem Sie eine der folgenden, abhängig von Ihrer geografische Region oder Instanz von Azure verknüpfen:
    - Für Nordamerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Für Europa:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Für Asien:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Für Lateinisch Nordamerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Für Japan:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Für Australien:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - Für [Azure Government](https://azure.microsoft.com/features/gov/)verwendet der US-Regierung Instanz von Azure aus:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Für Kanada:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Für Deutschland:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Für Indien:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Die Software Thales umfasst Python am %NFAST_HOME%\python\bin

2.  Bestätigen, dass die Überprüfung erfolgreichen angibt, Folgendes angezeigt: **Ergebnis: Erfolg**

Dieses Skript überprüft, ob die Kette des Unterzeichners auf die Thales Stamm-Taste. Der Stammschlüssel-Hash in das Skript eingebettet ist, und der Wert sollte **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**sein. Sie können diesen Wert auch separat des Besuchs der [Website Thales](http://www.thalesesec.com/)bestätigen.

Sie nun können einen neuen Product Key zu erstellen.

###<a name="step-33-create-a-new-key"></a>Schritt 3.3: Erstellen eines neuen Product Keys

Erstellt einen Registrierungsschlüssel mit dem Thales **Generatekey** Programm an.

Führen Sie den folgenden Befehl zum Generieren des Schlüssels ein:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Wenn Sie diesen Befehl ausführen, gehen Sie folgendermaßen vor:

- Die Parameter *schützen* müssen mit dem Wert **Modul**festgelegt werden, wie dargestellt. Dies erstellt einen Schlüssel Modul geschützt. Die BYOK Extras unterstützt OCS geschützt Tasten nicht.

- Ersetzen Sie den Wert von *Contosokey* für die **Ident** und **Plainname** mit einem beliebigen Zeichenfolgenwert aus. Zum Minimieren Verwaltungsaufwand und Fehler vermeiden, empfehlen wir, dass Sie den gleichen Wert für beide verwenden. Der Wert **Ident** muss nur Zahlen, Striche und Kleinbuchstaben enthalten.

- Die Pubexp bleibt leer (Standard) in diesem Beispiel, aber Sie können angeben, dass bestimmte Werte. Weitere Informationen finden Sie in der Dokumentation Thales.

Dieser Befehl erstellt eine Token Key-Datei im Ordner "%NFAST_KMDATA%\local" mit einem Namen beginnend mit **Key_simple_**, gefolgt von dem **Ident** , die im Befehl angegeben wurde. Beispiel: **Key_simple_contosokey**. Diese Datei enthält einen verschlüsselten Schlüssel.

Sichern Sie diese Token Schlüsseldatei an einem sicheren Ort.

>[AZURE.IMPORTANT] Wenn Sie den Key später zum Azure-Taste Tresor übertragen, kann nicht Microsoft Schlüssel für Sie exportieren, sodass es äußerst wichtig ist, dass Sie Ihre Schlüssel und Wertpapiers Welt sicheres sichern. Wenden Sie sich an Thales für Leitfäden und bewährte Methoden für den Key sichern.

Sie können nun den Key in Azure-Taste Tresor übertragen.

##<a name="step-4-prepare-your-key-for-transfer"></a>Schritt 4: Vorbereiten des Schlüssels für die Übertragung

Führen Sie für diesen vierter Schritt die folgenden Verfahren auf dem Computer getrennt ein.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Schritt 4.1: Erstellen einer Kopie Ihres Schlüssels mit eingeschränkten Berechtigungen

Führen Sie eine der folgenden, abhängig von Ihrer geografische Region oder Instanz von Azure zum Verringern der Berechtigungen für den Key von einer Befehlszeile aus:

- Für Nordamerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Für Europa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Für Asien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Für Lateinisch Nordamerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Für Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Für Australien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- Für [Azure Government](https://azure.microsoft.com/features/gov/)verwendet der US-Regierung Instanz von Azure aus:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Für Kanada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Für Deutschland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Für Indien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Wenn Sie diesen Befehl ausführen, ersetzen Sie *Contosokey* mit demselben Wert im angegebenen **Schritt 3.3: Erstellen Sie einen neuen Product Key** aus dem [Generieren des Keys](#step-3-generate-your-key) Schritt.

Sie werden aufgefordert, Ihre Wertpapiers Welt Administrator Karten einstecken.

Wenn der Befehl abgeschlossen ist, Sie sehen **Ergebnis: Erfolg** und die Kopie Ihres Schlüssels mit eingeschränkten Berechtigungen sind in der Datei mit dem Namen Key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Schritt 4.2: Prüfen Sie die neue Kopie der Schlüssel

Optional führen Sie aus, die Thales Dienstprogramme, um die minimalen Berechtigungen für den neuen Schlüssel bestätigen:

- aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- Kmfile-Dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Wenn Sie diese Befehle ausführen, ersetzen Sie Contosokey mit demselben Wert im angegebenen **Schritt 3.3: Erstellen Sie einen neuen Product Key** aus dem [Generieren des Keys](#step-3-generate-your-key) Schritt.

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Schritt 4.3: Verschlüsseln Sie den Key mithilfe von Microsoft Exchange-Schlüssel

Führen Sie einen der folgenden Befehle, je nach geografische Region oder Azure-Instanz aus:

- Für Nordamerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Europa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Asien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Lateinisch Nordamerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Australien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für [Azure Government](https://azure.microsoft.com/features/gov/)verwendet der US-Regierung Instanz von Azure aus:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Kanada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Deutschland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Indien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Wenn Sie diesen Befehl ausführen, gehen Sie folgendermaßen vor:

- Ersetzen Sie *Contosokey* durch den Bezeichner enthält, die Sie zum Generieren des Schlüssels in verwendet **Schritt 3.3: Erstellen Sie einen neuen Product Key** aus dem [Generieren des Keys](#step-3-generate-your-key) Schritt.

- Ersetzen Sie *SubscriptionID* durch die ID des Azure-Abonnements, die Ihrem Key Tresor enthält. Dieser Wert wiederhergestellte zuvor, in **Schritt 1.2: Abrufen Ihrer Azure-Abonnement-ID** aus dem [Vorbereiten Ihrer Internet verbundenen Arbeitsstationen](#step-1-prepare-your-internet-connected-workstation) Schritt.

- Ersetzen Sie *ContosoFirstHSMKey* mit einer Beschriftung, die für Ihre Ausgabedateinamen verwendet wird.

Wenn dies erfolgreich abgeschlossen wurde, zeigt **Ergebnis: Erfolg** und es wird eine neue Datei im aktuellen Ordner mit dem folgenden Namen: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Schritt 4,4: Kopieren von Ihrem Paket Key Transfer in das Internet verbundenen Arbeitsstationen

Verwenden Sie ein USB-Laufwerk oder anderen tragbaren Speicher um die Ausgabedatei aus dem vorherigen Schritt (KeyTransferPackage-ContosoFirstHSMkey.byok) in der Internet verbundenen Arbeitsstationen zu kopieren.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Schritt 5: Übertragen Sie Ihre Schlüssel zum Azure-Taste Tresor

Verwenden Sie das [Hinzufügen-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) -Cmdlet für diese im letzten Schritt auf dem Internet verbundenen das Paket Key Transfer hochladen, das Sie in der getrennt Arbeitsstationen an das Azure-Taste Tresor HSM kopiert haben:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Wenn der Upload erfolgreich ist, finden Sie die Eigenschaften des Schlüssels, die Sie soeben hinzugefügt haben angezeigt.


##<a name="next-steps"></a>Nächste Schritte

Sie können jetzt HSM geschützte Schlüssel in Ihrem Key Tresor verwenden. Weitere Informationen finden Sie im Abschnitt **, wenn Sie ein Hardwaresicherheitsmodul (HSM) verwenden möchten** in das [Erste Schritte mit Azure Schlüssel Tresor](key-vault-get-started.md) Lernprogramm.
