## <a name="using-azure-portal"></a>Verwenden von Azure-portal

1. Wählen Sie den virtuellen Computer erneut bereitgestellt werden sollen, und klicken Sie auf die Schaltfläche 'Bereitstellen' in 'Einstellungen' Blades. Führen Sie einen Bildlauf nach unten zum Abschnitt **Support und Problembehandlung** finden Sie unter, der die Schaltfläche 'Bereitstellen' wie im folgenden Beispiel enthält:

    ![Azure virtueller Computer blade](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)

2. Klicken Sie auf die Schaltfläche 'Erneut bereitstellen', um den Vorgang zu bestätigen:

    ![Erneut bereitstellen einer Blades virtueller Computer](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)

3. Der **Status** des virtuellen Computer ändert sich zum *Aktualisieren* als den virtuellen Computer vorbereitet erneut, wie im folgenden Beispiel bereitgestellt:

    ![Virtueller Computer aktualisieren](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)

4. Der **Status** nimmt dann *Starten* , wenn der virtuellen Computer auf einem neuen Azure Host, wie im folgenden Beispiel gezeigt startet:

    ![Virtueller Computer starten](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)

5. Nach der virtuellen Computer die, nach Abschluss des Bootvorgangs den **Status** gibt klicken Sie dann auf *Ausführen*, hat, der angibt, des virtuellen Computer erfolgreich erneut bereitgestellt wurde:

    ![Virtueller Computer Ausführung](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)