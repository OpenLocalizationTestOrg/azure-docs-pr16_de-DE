Ein Azure Lastenausgleich ist ein Lastenausgleich Layer 4 (TCP, UDP). Lastenausgleich bietet hohe Verfügbarkeit durch eingehenden Datenverkehr zwischen Dienstinstanzen fehlerfrei, in der Cloud Services oder virtuellen Computern in einer Gruppe von laden Lastenausgleich verteilen. Azure Lastenausgleich können auch Dienste auf mehrere Ports, mehrere IP-Adressen oder beides präsentieren.

Sie können ein Lastenausgleich zu konfigurieren:

* Laden Sie Saldo eingehenden Internetdatenverkehr virtuellen Maschinen (virtuelle Computer). Wir bezeichnen ein Lastenausgleich in diesem Szenario als eine [Internet zugänglichen Lastenausgleich](../articles/load-balancer/load-balancer-internet-overview.md).
* Laden Saldo Datenverkehr zwischen virtuellen Computern in einem virtuellen Netzwerk (VNet), zwischen virtuellen Computern in der Cloud Services oder zwischen dem lokalen Computer und virtuellen Computern in einem Cross lokale virtuelle Netzwerk. Wir bezeichnen ein Lastenausgleich in diesem Szenario als ein [Interner Lastenausgleich (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Weiterleiten von externen Datenverkehr auf eine bestimmte Instanz des virtuellen Computer.
