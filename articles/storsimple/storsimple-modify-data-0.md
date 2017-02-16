<properties 
   pageTitle="Ändern Sie die Daten 0 Einstellungen auf einem Gerät StorSimple | Microsoft Azure"
   description="Informationen Sie zum Verwenden von Windows PowerShell für StorSimple, um die auf Ihrem Gerät StorSimple Schnittstelle 0 Netzwerk neu zu konfigurieren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-device"></a>Ändern Sie die Daten 0 Netzwerk Benutzeroberflächen-Einstellungen auf Ihrem Gerät StorSimple

## <a name="overview"></a>(Übersicht)

Ihr Gerät Microsoft Azure StorSimple verfügt über sechs Netzwerk-Schnittstellen aus Daten 0 zu Daten 5. Die Daten 0 Benutzeroberfläche immer über die Windows PowerShell-Benutzeroberfläche oder die serielle Konsole konfiguriert ist, und automatisch Cloud-aktiviert ist. Beachten Sie, dass Sie nicht Daten 0 Netzwerk-Benutzeroberfläche über das Azure klassischen Portal konfigurieren können. 

Die Daten 0 Schnittstelle zuerst, durch den Setup-Assistenten während konfiguriert ist Anfangsbuchstaben Bereitstellung des Geräts StorSimple. Wenn das Gerät in Betrieb Modus ist, möglicherweise müssen Sie die Daten 0 umkonfigurieren Einstellungen. In diesem Lernprogramm bietet zwei Methoden zum Ändern von Daten 0 Einstellungen beide bis Windows PowerShell für StorSimple Netzwerk.

Nach dem Lesen dieses Lernprogramms, werden Sie können:

- Ändern von Daten 0 Netzwerk-Einstellung durch den Setup-Assistenten
- Ändern von Einstellungen für 0 Netzwerk Daten über die `Set-HcsNetInterface` Cmdlet



## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Ändern von Einstellungen für 0 Netzwerk Daten über Setup-Assistenten
Sie können Daten 0 Netzwerkeinstellungen durch Herstellen einer Verbindung mit der Windows PowerShell-Benutzeroberfläche von Ihrem Gerät StorSimple und Starten einer Sitzung für Setup-Assistenten neu konfigurieren. Gehen Sie folgendermaßen vor, um Daten 0 ändern Einstellungen:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>So ändern Sie die Daten 0 Netzwerkeinstellungen über Setup-Assistenten

1. Wählen Sie im Menü seriellen Konsole Option 1, **Melden Sie sich mit Vollzugriff**aus. Wenn Sie aufgefordert werden das **Gerät Administratorkennwort**angeben. Das standardmäßige Kennwort lautet `Password1`.

2. Geben Sie an der Befehlszeile ein:

    `Invoke-HcsSetupWizard`

3. Setup-Assistent wird angezeigt, helfen Ihnen die Konfiguration der Daten 0 Benutzeroberfläche von Ihrem Gerät. Neue Werte für die IP-Adresse, Gateway und Netmask angeben.

> [AZURE.NOTE] Die feste Controller IP-Adressen werden über die Seite **Konfigurieren** des Geräts StorSimple im klassischen Azure-Portal neu konfiguriert werden müssen. Wechseln Sie weitere Informationen zu [Netzwerk-Schnittstellen ändern](storsimple-modify-device-config.md#modify-network-interfaces).


## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Ändern von Einstellungen für Daten Netzwerk 0 bis Cmdlet "Set-HcsNetInterface"
Eine alternative Möglichkeit, Daten 0 umkonfigurieren Netzwerk-Benutzeroberfläche durch die Verwendung von wird der `Set-HcsNetInterface` Cmdlet. Aus der Windows PowerShell-Benutzeroberfläche von Ihrem Gerät StorSimple wird das Cmdlet ausgeführt. Wenn Sie dieses Verfahren verwenden zu können, kann der Controller feste IP-Adressen auch so konfiguriert werden. Gehen Sie folgendermaßen vor, um die Daten 0 ändern Einstellungen: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>So ändern Sie die Daten 0 Netzwerkeinstellungen durch das Cmdlet "Set-HcsNetInterface"

1. Wählen Sie im Menü seriellen Konsole Option 1, **Melden Sie sich mit Vollzugriff**aus. Wenn Sie aufgefordert werden das Administratorkennwort Gerät bereitstellen. Das standardmäßige Kennwort lautet `Password1`.

2. Geben Sie an der Befehlszeile ein:

    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
    
    Geben Sie die folgenden Werte in der spitzen Klammern für Daten 0:
                                            
    - IPv4-Adresse
    
    - IPv4-gateway
    
    - IPv4-Subnetzmaske
    
    - Feste IPv4-Adresse für Controller 0

    - Feste IPv4-Adresse für Controller 1

    Weitere Informationen über die Verwendung dieses Cmdlets wechseln Sie zu [Windows PowerShell für StorSimple Cmdlet Bezug](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Nächste Schritte

- Um Netzwerk-Schnittstellen als Daten 0 konfigurieren, können Sie die [Seite in der klassischen Azure-Portal konfigurieren](storsimple-modify-device-config.md). 

- Wenn Sie Probleme auftreten, wenn Sie Ihre Netzwerk-Schnittstellen konfigurieren, lesen Sie [Behandeln von Problemen bei der Bereitstellung](storsimple-troubleshoot-deployment.md).

