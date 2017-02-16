Beim Hinzufügen von Daten Datenträger einer Linux VM können Fehler auftreten, wenn ein Datenträger am LUN 0 nicht vorhanden ist. Wenn Sie einen Datenträger mit manuell hinzufügen, die `azure vm disk attach-new` Befehl, und legen Sie eine LUN (`--lun`) möglichst der Azure-Plattform die entsprechende LUN bestimmen, achten Sie darauf, dass ein Datenträger bereits vorhanden ist / bei LUN 0 vorhanden sind. 

Betrachten Sie das folgende Beispiel mit einem Codeausschnitt der Ausgabe von `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

Die beiden Datenträger am LUN 0 und 1 LUN vorhanden (die erste Spalte in der `lsscsi` ausgeben Details `[host:channel:target:lun]`). Beide Datenträger sollten Accessbile aus, in dem virtuellen Computer. Wenn Sie den ersten Datenträger zu LUN 1 hinzugefügt werden sollen, und die zweite Festplatte bei LUN 2 manuell festgelegt, möglicherweise nicht die Datenträger richtig aus innerhalb Ihrer virtuellen Computer angezeigt werden.

> [AZURE.NOTE] Die Azure `host` Wert ist 5 in diesen Beispielen, aber dies kann variieren je nach Typ Speicher, die Sie auswählen.

Dieses Verhalten der Datenträger ist nicht an einer Azure Problem, doch die Möglichkeit, in der der Linux Kernel die SCSI-Spezifikationen folgt. Wenn der Linux Kernel den SCSI-Bus für angeschlossenen Geräte scannt, muss ein Gerät bei LUN 0 in Reihenfolge für das System weiterhin für weitere Geräte scannen gefunden werden. Als solche:

- Überprüfen Sie die Ausgabe der `lsscsi` nach dem Hinzufügen eines Datenträgers Daten, um sicherzustellen, dass Sie einen Datenträger mit LUN 0 ist.
- Wenn Ihre Datenträger nicht richtig in Ihrem virtuellen Computer angezeigt wird, überprüfen Sie, ob ein Datenträger am LUN 0 vorhanden ist.