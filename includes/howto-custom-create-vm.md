#<a name="how-to-create-a-custom-virtual-machine"></a>So erstellen Sie einen benutzerdefinierten virtuellen Computern

Eine *benutzerdefinierte* virtuellen Computern bezieht sich auf einer virtuellen Computern, die Sie mithilfe der **Aus der Katalog** -Methode erstellen, da sie weitere Optionen als die Methode zum **Schnellen Erstellen** konfigurieren können. Diese Optionen umfassen Folgendes:

- Weitere Auswahlmöglichkeiten für das Bild zu verwenden, um die Erstellung des virtuellen Computers (virtueller Computer)
- Herstellen einer Verbindung den virtuellen Computer zu einem virtuellen Netzwerk
- Hinzufügen von den virtuellen Computer zu einem vorhandenen Clouddienst
- Hinzufügen von den virtuellen Computer auf einen Satz Verfügbarkeit

> [AZURE.IMPORTANT] Wenn Sie möchten Ihre virtuellen Computern mit einem virtuellen Netzwerk, sodass Sie direkt nach Hostname oder Einrichten von Cross lokale Verbindungen herstellen können, stellen Sie sicher, dass Sie beim Erstellen des virtuellen Computers das virtuelle Netzwerk angeben. Eine virtuellen Computern kann ein virtuelles Netzwerk beitreten nur bei der Erstellung des virtuellen Computers konfiguriert werden. Weitere Informationen über virtuelle Netzwerke finden Sie unter [Azure virtuelle Network (Übersicht)](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Melden Sie sich mit dem [Azure-Portal](http://manage.windowsazure.com)aus.

2. Klicken Sie auf der Befehlsleiste auf **neu**.

3. Klicken Sie auf **berechnen**, klicken Sie auf **virtuellen Computern**, und klicken Sie dann auf **Aus Galerie**.

4. Wählen Sie das Bild, das Sie verwenden möchten, und klicken Sie dann auf den Pfeil, um den Vorgang fortzusetzen.

5. Wenn mehrere Versionen eines Bilds in **Datum der Version Freigabe**verfügbar sind, wählen Sie die Version, die Sie verwenden möchten.

6. Geben Sie im Feld **Name des virtuellen Computers**den Namen, den Sie für den virtuellen Computer verwenden möchten.

7. Verwenden Sie **Ebene** und den **Schriftgrad** , um die entsprechende Größe des virtuellen Computers auszuwählen. Die Größe, die Sie auswählen, wirkt sich auf die maximale Konfiguration des virtuellen Computers als auch die Preise. Finden Sie unter Details zur Konfiguration des [virtuellen Computers und Cloud-Dienst Größen für Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. **Neuen Benutzernamen ein**Geben Sie einen Namen für das Administratorkonto, das zur Verwaltung des Servers verwenden möchten.

9. Geben Sie im Feld **Neues Kennwort**für das Administratorkonto ein sicheres Kennwort ein. Geben Sie im Feld **Kennwort bestätigen**erneut dasselbe Kennwort.

10. Klicken Sie auf den Pfeil, um den Vorgang fortzusetzen.

11. Führen Sie in der **Cloud-Dienst**eine der folgenden Aktionen aus:

    - Ist dies das erste oder nur virtuellen Computern in der Cloud-Dienst, wählen Sie **Erstellen Sie einen neuen Cloud-Dienst aus** Klicken Sie dann in der **Cloud Service DNS-Name**, geben Sie einen Namen, der zwischen 3 und 24 Kleinbuchstaben und Zahlen verwendet ein. Dieser Name wird Teil des URIS, die zum Kontaktieren des virtuellen Computers über den Clouddienst verwendet wird.
    - Wenn dieses virtuellen Computers auf einen Clouddienst hinzugefügt wird, wählen Sie ihn in der Liste aus.

    > [AZURE.NOTE] Weitere Informationen über das Platzieren von virtuellen Computern in der gleichen Cloud-Dienst finden Sie unter [Herstellung der Verbindung virtuellen Computern in einen Cloud-Dienst](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. Wählen Sie in der **Region Zugehörigkeit/Gruppe/virtuellen Netzwerk**Region, Zugehörigkeit Gruppe oder virtuelle Netzwerk aus, das Sie für den virtuellen Computer verwenden möchten. Weitere Informationen zu Gruppen die finden Sie unter [Informationen zu Gruppen für virtuelle Netzwerk](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. **Speicher-Konto**wählen Sie ein vorhandenes Speicherkonto für die virtuelle Festplatte-Datei aus, oder verwenden Sie ein automatisch generiertes Speicherkonto. Je nach Region nur eine Speicher-Konto wird automatisch erstellt. In diesem Speicherkonto befinden sich alle anderen virtuellen Computern, die Sie mit dieser Einstellung erstellen. Sie sind auf 20 Speicherkonten beschränkt.

14. Wenn Sie des virtuellen Computers gehören soll eine Menge Verfügbarkeit in **Verfügbarkeit festlegen**möchten, wählen Sie **Erstellen Verfügbarkeit festlegen** oder Lizenz zu einem bestehenden Satz von Verfügbarkeit.

    **Hinweis**: virtuellen Computern in einem Satz Verfügbarkeit zu anderen Fehlerstrukturanalyse Domänen bereitgestellt werden. Platzieren von mehreren virtuellen Computern in einer Verfügbarkeit festlegen wird sichergestellt, dass eine Anwendung Netzwerkfehlern, lokalen Festplatte Hardwarefehlern und geplanten Ausfallzeit verfügbar ist.

15.  Überprüfen Sie unter **Endpunkte**die neuen Endpunkte, die Verbindungen des virtuellen Computers, beispielsweise über Remote Desktop oder ein Client Secure Shell (SSH) zulässt erstellt wird. Außerdem können Sie Endpunkte jetzt hinzufügen oder später zu erstellen. Erstellen sie später Anweisungen finden Sie unter [Festlegen von Endpunkte eines virtuellen Computers](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  Klicken Sie unter **Virtueller Computer Agent**entscheiden Sie können, ob der Agent virtueller Computer installieren. Dieser Agent stellt die Umgebung für Erweiterungen zu installieren, die Sie interagieren mit den virtuellen Computern helfen können. Details finden Sie unter [Verwalten von Erweiterungen](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Klicken Sie auf den Pfeil, um die Erstellung des virtuellen Computers.

    ![Benutzerdefinierte virtuellen Computern Erstellung erfolgreich](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Nächste Schritte##
Nachdem der virtuellen Computern erstellt wurde, wird es automatisch gestartet. Wenn das Portal den Status als aktiv angezeigt wird, können Sie mit dem virtuellen Computer anmelden. Anweisungen finden Sie unter den folgenden Artikeln:

- [So melden Sie sich an einen virtuellen Computer mit Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [So melden Sie sich bei einem virtuellen Computern unter WindowsServer](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

