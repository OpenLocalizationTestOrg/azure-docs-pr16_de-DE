<properties
    pageTitle="Erster Blick: Schützen Azure-virtuellen Computern mit einer Sicherungskopie Tresor | Microsoft Azure"
    description="Schützen von Azure-virtuellen Computern mit Sicherung Tresor. Lernprogramm erfahren Sie, Tresor anlegen, virtuellen Computern registrieren, Richtlinie erstellen und Schützen von virtuellen Computern in Azure."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/15/2016"
    ms.author="markgal; jimpark"/>


# <a name="first-look-backing-up-azure-virtual-machines"></a>Suchen Sie zuerst: Sichern Azure-virtuellen Computern

> [AZURE.SELECTOR]
- [Schützen von virtuellen Computern mit einer Wiederherstellung Services Tresor](backup-azure-vms-first-look-arm.md)
- [Schützen Sie mit einer Sicherungskopie Tresor Azure-virtuellen Computern](backup-azure-vms-first-look.md)

In diesem Lernprogramm gelangen Sie durch die Schritte zum Sichern einer Azure-virtuellen Computern (virtueller Computer) zu einer Sicherung Tresor in Azure. Dieser Artikel beschreibt das Modell Classic oder Dienst-Manager Bereitstellungsmodell für Sichern virtueller Computer an. Wenn Sie Sichern eines virtuellen Computers zu einem Tresor Wiederherstellung Dienste interessiert, die zu einer Ressourcengruppe gehört sind, finden Sie unter [zuerst aussehen: schützen virtueller Computer mit einer Wiederherstellung Services Tresor](backup-azure-vms-first-look-arm.md). Damit dieses Lernprogramm erfolgreich abgeschlossen, müssen diese erforderlichen Komponenten vorhanden sein:

- Sie haben einen virtuellen in Ihrem Abonnement Azure erstellt.
- Der virtuellen Computer ein Connectivity Azure öffentlicher IP-Adressen ist. Weitere Informationen finden Sie unter [Netzwerkkonnektivität](./backup-azure-vms-prepare.md#network-connectivity).

Um einen virtuellen Computer zu sichern, gibt es fünf Hauptschritte:  

![Schritt 1-](./media/backup-azure-vms-first-look/step-one.png) Erstellen einer Sicherungskopie Tresor oder benennen Sie eine vorhandene Sicherung Tresor. <br/>
![Schritt 2 –](./media/backup-azure-vms-first-look/step-two.png) klassischen Azure-Portal zu ermitteln und registriert die virtuellen Computer verwenden. <br/>
![Schritt 3-](./media/backup-azure-vms-first-look/step-three.png) Agent virtueller Computer installieren. <br/>
![Schritt vier](./media/backup-azure-vms-first-look/step-four.png) die Richtlinie für den Schutz von den virtuellen Computern erstellen. <br/>
![Schritt 5 –](./media/backup-azure-vms-first-look/step-five.png) die Sicherung ausführen.

![Allgemeine Übersicht über Sicherung Prozess virtueller Computer](./media/backup-azure-vms-first-look/backupazurevm-classic.png)

>[AZURE.NOTE] Azure weist zwei Bereitstellungsmodelle für das Erstellen von und Arbeiten mit Ressourcen: [Ressourcenmanager und Classic](../resource-manager-deployment-model.md). In diesem Lernprogramm ist für die Verwendung mit den virtuellen Computern, die in der klassischen Azure-Portal erstellt werden können. Der Sicherung von Azure-Dienst unterstützt Ressourcenmanager-basierten virtuellen Computern. Details zum Sichern von virtuellen Computern in einer Wiederherstellungsdatei Services Tresor finden Sie unter [First Look: schützen virtueller Computer mit einer Wiederherstellung Services Tresor](backup-azure-vms-first-look-arm.md).



## <a name="step-1---create-a-backup-vault-for-a-vm"></a>Schritt 1 – Erstellen einer Sicherungskopie Tresor für einen virtuellen Computer

Eine Sicherung Tresor ist eine Entität, die speichert die Sicherung und Wiederherstellungspunkte, die über einen Zeitraum erstellt wurden. Der Sicherung Tresor enthält auch die Sicherung Richtlinien, die mit den virtuellen Computern zu sichernden angewendet werden.

1. Melden Sie sich mit dem [klassischen Azure-Portal](http://manage.windowsazure.com/)aus.

2. Klicken Sie auf **neu** , klicken Sie in der unteren linken Ecke des Portals Azure

    ![Klicken Sie auf Menü ' Neu '](./media/backup-azure-vms-first-look/new-button.png)

3. In den Assistenten zum schnellen erstellen, klicken Sie auf **Data Services** > **Wiederherstellung Services** > **Sicherung Tresor** > **Symbolleiste erstellen**.

    ![Erstellen Sie zusätzliche Tresor](./media/backup-azure-vms-first-look/new-vault-wizard-one-subscription.png)

    Der Assistent fordert Sie für den **Namen** und **Region**. Wenn Sie mehrere Abonnements verwalten, wird ein Dialogfeld zur Auswahl des Abonnements angezeigt.

4. Geben Sie für den **Namen**einen Anzeigenamen ein, um den Tresor zu identifizieren. Der Name muss für das Abonnement Azure eindeutig sein.

5. Wählen Sie in der **Region**das geografische Region für den Tresor ein. Tresor der **müssen** werden in der gleichen Region als den virtuellen Computern geschützten.

    Wenn Sie die Region wissen nicht, in der Ihre virtuellen Computer vorhanden ist, schließen Sie den Assistenten, und klicken Sie auf **virtuellen Computern** in der Liste der Azure-Dienste. Die Spalte Speicherort enthält den Namen des Bereichs. Wenn Sie mehrere Bereiche virtuellen Computern haben, erstellen Sie eine Sicherungskopie Tresor in jeder Region.

6. Ist es keine **Abonnement** Dialogfeld im Assistenten, fahren Sie mit dem nächsten Schritt fort. Wenn Sie mit mehreren Abonnements arbeiten, wählen Sie ein Abonnement, mit dem neuen Sicherung Tresor zugeordnet werden soll.

    ![Erstellen von Tresor Spruch Benachrichtigung](./media/backup-azure-vms-first-look/backup-vaultcreate.png)

7. Klicken Sie auf **Tresor erstellen**. Es dauert eine Weile für die Sicherungsdatei Tresor erstellt werden. Überwachen Sie die Benachrichtigungen Status am unteren Rand des Portals.

    ![Erstellen von Tresor Spruch Benachrichtigung](./media/backup-azure-vms-first-look/create-vault-demo.png)

    Eine Meldung bestätigt, dass der Tresor erfolgreich erstellt wurde. Es wird auf der Seite **Wiederherstellung Dienste** als **aktiv**aufgeführt.

    ![Erstellen von Tresor Spruch Benachrichtigung](./media/backup-azure-vms-first-look/create-vault-demo-success.png)

8. Wählen Sie auf der Seite **Wiederherstellung Dienste** +++ Liste den Sie erstellt haben, den Sie zum Starten der Seite **Schnellstart** Tresor.

    ![Liste der Sicherungsdatei +++](./media/backup-azure-vms-first-look/active-vault-demo.png)

9. Klicken Sie auf der Seite **Schnellstart** auf **Konfigurieren** , um die Option Speicher Replikation zu öffnen.
    ![Liste der Sicherungsdatei +++](./media/backup-azure-vms-first-look/configure-storage.png)

10. Wählen Sie auf die Option **Speicherreplikation** die Replikationsoption für den Tresor aus.

    ![Liste der Sicherungsdatei +++](./media/backup-azure-vms-first-look/backup-vault-storage-options-border.png)

    Standardmäßig weist Ihrem Tresor Geo redundante Speicherung. Wählen Sie Geo redundante Speicherung ist dies die primäre Sicherung aus. Wählen Sie Lokales redundante Speicherung aus, wenn Sie eine Option kostengünstigere wünschen, die nicht ganz als dauerhaften ist. Weitere Informationen zum Geo redundante und lokal redundante Speicheroptionen in der [Übersicht über die Replikation Azure-Speicher](../storage/storage-redundancy.md).

Nach dem Auswählen der Option Speicherplatz für den Tresor, sind Sie bereit sind, den virtuellen Computer mit dem Tresor zugeordnet werden soll. Um die Zuordnung zu beginnen, ermitteln und Azure-virtuellen Computern registrieren.

## <a name="step-2---discover-and-register-azure-virtual-machines"></a>Schritt 2: ermitteln und registrieren Azure-virtuellen Computern
Führen Sie vor der Registrierung des virtuellen Computer mit einem Tresor den Erkennungsvorgang, um alle neuen virtuellen Computern zu identifizieren. Dies gibt eine Liste von virtuellen Computern in das Abonnement, zusammen mit weiteren Informationen, wie Sie den Namen der Cloud-Dienst und die Region ein.

1. Melden Sie sich bei der [klassischen Azure-portal](http://manage.windowsazure.com/)

2. Klicken Sie im Portal Azure klassischen auf **Wiederherstellung Services** , um die Liste der Wiederherstellung Services +++ öffnen.
    ![Wählen Sie die Arbeitsbelastung](./media/backup-azure-vms-first-look/recovery-services-icon.png)

3. Wählen Sie aus der Liste der +++ den Tresor eines virtuellen Computers zu sichern.

    Wenn Sie Ihrem Tresor auswählen, auf der Seite **Schnellstart** geöffnet

4. Klicken Sie im Menü Tresor auf **Elemente registriert**.

    ![Wählen Sie die Arbeitsbelastung](./media/backup-azure-vms-first-look/configure-registered-items.png)

5. Wählen Sie im Menü **Typ** **Azure-virtuellen Computern**.

    ![Wählen Sie die Arbeitsbelastung](./media/backup-azure-vms/discovery-select-workload.png)

6. Klicken Sie auf **Suchen** , am unteren Rand der Seite.
    ![Ermitteln der Schaltfläche](./media/backup-azure-vms/discover-button-only.png)

    Der Erkennungsvorgang kann einige Minuten dauern, während der virtuellen Computern tabellarisch angeordnet werden, sind. Es gibt eine Benachrichtigung am unteren Rand des Bildschirms, mit dem Sie wissen, dass der Prozess ausgeführt wird.

    ![Ermitteln von virtuellen Computern](./media/backup-azure-vms/discovering-vms.png)

    Führen Sie die Änderungen Benachrichtigung, wenn der Vorgang ist.

    ![Suche abgeschlossen](./media/backup-azure-vms-first-look/discovery-complete.png)

7. Klicken Sie auf **Registrieren** , am unteren Rand der Seite.
    ![Schaltfläche "Register"](./media/backup-azure-vms-first-look/register-icon.png)

8. Wählen Sie im Kontextmenü **Registrieren Elemente** den virtuellen Computern, die Sie erfassen möchten.

    >[AZURE.TIP] Mehrere virtuelle Computer werden gleichzeitig erfasst.

    Ein Auftrag wird für jeden virtuellen Computer erstellt, die Sie ausgewählt haben.

9. Klicken Sie auf **Ansicht Position** in der Benachrichtigung, um die Seite **Aufträge** anzuzeigen.

    ![Register-Position](./media/backup-azure-vms/register-create-job.png)

    Des virtuellen Computers wird auch in der Liste der registrierten Elemente und außerdem den Status des Vorgangs Registrierung.

    ![Registrieren Status 1](./media/backup-azure-vms/register-status01.png)

    Wenn der Vorgang abgeschlossen ist, der Status ändert sich in den Zustand *registriert* wiederzugeben.

    ![Registrierungsinformationen Status 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---install-the-vm-agent-on-the-virtual-machine"></a>Schritt 3: Installieren des virtuellen Computer-Agents auf den virtuellen Computern

Klicken Sie auf der Azure-virtuellen Computern für die Sicherung Erweiterung entwickelt muss der Azure-virtuellen Computer-Agent installiert sein. Wenn Ihre virtuellen Computer aus dem Katalog Azure erstellt wurde, ist der virtuellen Computer-Agent bereits vorhanden des virtuellen Computers. Sie können den [Schutz Ihrer virtuellen Computern](backup-azure-vms-first-look.md#step-4-protect-azure-virtual-machines)überspringen.

Wenn Ihre virtuellen Computer aus einem lokalen Rechenzentrum migriert, verfügt der virtuellen Computer wahrscheinlich nicht der virtuellen Computer-Agent installiert. Sie müssen den virtuellen Computer-Agent des virtuellen Computers, bevor Sie mit den virtuellen Computer schützen installieren. Die detaillierten Schritte zum Installieren des virtuellen Computer-Agents finden Sie im [Abschnitt virtueller Computer Agent Artikel Sicherung virtuellen Computern](backup-azure-vms-prepare.md#vm-agent).


## <a name="step-4---create-the-backup-policy"></a>Schritt 4: Erstellen der Sicherungsdatei Richtlinie
Bevor Sie den anfänglichen Sicherung Auftrag ausgelöst wird, legen Sie den Zeitplan, wenn Sicherung Momentaufnahmen geöffnet werden. Das Planen, wann die Sicherung Momentaufnahmen werden entnommen und die Zeitdauer dieser Momentaufnahmen aufbewahrt werden, ist die Sicherung Richtlinie. Die Aufbewahrung Informationen basiert auf Großvater-Vater-Sohn Sicherung Drehung des Farbschemas.

1. Navigieren Sie zu der Sicherungsdatei Tresor klicken Sie unter **Wiederherstellung Dienste** im klassischen Azure-Portal, und klicken Sie auf **Elemente registriert**.
2. Wählen Sie **Azure-virtuellen Computern** aus dem Dropdownmenü aus.

    ![Wählen Sie die Arbeitsbelastung-Portal](./media/backup-azure-vms/select-workload.png)

3. Klicken Sie auf **schützen** am unteren Rand der Seite.
    ![Klicken Sie auf schützen](./media/backup-azure-vms-first-look/protect-icon.png)

    **Schützen von Elementen-Assistent** wird angezeigt, und Listen *nur* virtuellen Computern, die registriert und nicht geschützt werden.

    ![Konfigurieren von Schutz bei](./media/backup-azure-vms/protect-at-scale.png)

4. Wählen Sie den virtuellen Computern, die Sie schützen möchten.

    Wenn es gibt zwei oder mehr virtuellen Computern mit demselben Namen ein, anhand der Cloud-Dienst zwischen den virtuellen Computern unterscheiden.

5. Klicken Sie im Menü **Konfigurieren Schutz** wählen Sie eine vorhandene Richtlinie aus, oder erstellen Sie eine neue Richtlinie, um die virtuellen Computer schützen, die Sie identifiziert.

    Neue Sicherung Depots verfügen über eine Standardrichtlinie der Tresor zugeordnet. Dieser Richtlinie übernimmt eines jeden Abend snapshot täglich, und tägliche Momentaufnahme ist 30 Tage lang aufbewahrt. Jede zusätzliche Richtlinie kann mehrere virtuelle Computer zugeordnet haben. Des virtuellen Computers nur kann eine Richtlinie zugeordnet nacheinander jedoch.

    ![Schützen Sie mit der neuen Richtlinie](./media/backup-azure-vms/policy-schedule.png)

    >[AZURE.NOTE] Eine Sicherung Richtlinie enthält ein Schema Aufbewahrungsrichtlinien für den geplanten Sicherungskopien. Wenn Sie eine vorhandene Sicherung Richtlinie auswählen, werden Sie nicht die Aufbewahrung Optionen im nächsten Schritt ändern.

6. Definieren Sie auf **Aufbewahrungszeitraum** den täglichen, wöchentlichen, Monats- und Jahreskalender mit Bereich für die bestimmte zusätzliche Punkte ein.

    ![Virtuellen Computern wird mit Wiederherstellungspunkt gesichert.](./media/backup-azure-vms/long-term-retention.png)

    Aufbewahrungsrichtlinie gibt die Länge der für die Speicherung einer Sicherungskopie an. Sie können angeben, dass unterschiedliche Aufbewahrungsrichtlinien basierend auf, wenn die Sicherungsdatei stammt.

7. Klicken Sie auf **Aufträge** zum Anzeigen der Liste der Aufträge **Schutz konfigurieren** .

    ![Konfigurieren von Schutz Position](./media/backup-azure-vms/protect-configureprotection.png)

    Jetzt, da Sie die Richtlinie hergestellt haben, fahren Sie mit dem nächsten Schritt fort, und führen Sie die ursprüngliche Sicherung.

## <a name="step-5---initial-backup"></a>Schritt 5: Initiale Sicherung

Nachdem Sie ein virtuellen Computer mit einer Richtlinie geschützt ist, können Sie die Beziehung auf der Registerkarte **Geschützte Elemente** anzeigen. Bis die ursprüngliche Sicherung durchgeführt wird, zeigt den **Status des Schutz** als **geschützten - (Ausstehend initial Sicherung)**. Standardmäßig ist die erste geplante Sicherung die *ursprüngliche sichern*.

![Sicherung ausstehend](./media/backup-azure-vms-first-look/protection-pending-border.png)

So starten Sie die ursprüngliche Sicherung jetzt

1. Klicken Sie auf **Jetzt sichern** am unteren Rand der Seite, auf der Seite **Geschützte Elemente** .
    ![Symbol für jetzt sichern](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Die Sicherung Azure Service erstellt eine Sicherungskopie Position für die ursprüngliche Sicherung.

2. Klicken Sie auf der Registerkarte **Aufträge** , um die Liste der Aufträge anzuzeigen.

    ![Sicherung wird ausgeführt](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Wenn anfängliche Sicherung abgeschlossen ist, wird der Status des virtuellen Computers auf der Registerkarte **Geschützte Elemente** *geschützten*.

    ![Virtuellen Computern wird mit Wiederherstellungspunkt gesichert.](./media/backup-azure-vms/protect-backedupvm.png)

    >[AZURE.NOTE] Sichern von virtuellen Computern umfasst eine lokale. Sie können keine virtuellen Computern aus einer zweiten Region auf eine Sicherung Tresor in einem anderen Region sichern. Ja, muss für jede Azure Region mit virtuellen Computern, die gesichert werden müssen, mindestens eine Sicherung Tresor in diesem Bereich erstellt werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun einen virtuellen erfolgreich gesichert wurden, gibt es einige weitere Schritte, die von Interesse sein könnten. Der meisten logische Schritt ist mit Wiederherstellen von Daten in einen virtuellen vertraut. Es gibt jedoch Verwaltungsaufgaben, mit die Hilfe Sie die Grundlagen zum Schützen Ihrer Daten und Kosten zu minimieren.

- [Verwalten Sie und überwachen Sie Ihrer virtuellen Computern](backup-azure-manage-vms.md)
- [Wiederherstellen von virtuellen Computern](backup-azure-restore-vms.md)
- [Hinweise zur Problembehandlung](backup-azure-vms-troubleshoot.md)


## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben, oder es ist eine Features, die Sie enthalten, finden Sie unter möchten [uns Feedback zu senden](http://aka.ms/azurebackup_feedback).
