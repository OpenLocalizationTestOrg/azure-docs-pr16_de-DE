##<a name="tcp-settings-for-azure-vms"></a>TCP-Einstellungen für Azure-virtuellen Computern

Azure-virtuellen Computern kommunizieren mithilfe von [NAT] mit dem öffentlichen Internet[ nat] (Network Address Translation). NAT-Geräte weisen eine öffentliche IP-Adresse und den Port zu einer Azure-virtuellen Computer, gleicht, die virtuellen Computer herstellen ein Sockets für die Kommunikation mit anderen Geräten. Wenn Pakete parallelen durch die Sockets nach einer bestimmten Zeit zu beenden, das NAT-Gerät bricht die Zuordnung und Sockets ist kostenlos von anderen virtuellen Computern verwendet werden soll.

Dies ist eine allgemeine NAT Verhalten, Kommunikationsprobleme auf TCP-basierte Applikationen verursachen können, die erwarten ein Sockets hinter dem Timeout verwaltet werden. Es gibt zwei im Leerlauf Timeouteinstellungen für Sitzungen in einem Zustand *Verbindung hergestellt* berücksichtigen:

- **eingehende** bis [Azure Lastenausgleich][azure-lb-timeout]. Diese Timeout standardmäßig auf 4 Minuten, und kann auf 30 Minuten angepasst werden.
- **ausgehende** [SNAT] mit[ snat] (Quelle NAT). Dieses Timeout auf 4 Minuten festgelegt ist, und kann nicht angepasst werden.

Um sicherzustellen, dass Verbindungen die Beschränkung Timeout nicht verloren gegangen sind, sollten Sie sicher, dass entweder eine Anwendung hält die Sitzung aktiv, oder Sie können das zugrunde liegenden Betriebssystem dazu konfigurieren vornehmen. Die Einstellungen zu verwendenden unterscheiden sich für Linux und Windows-Betriebssysteme, wie unten dargestellt.

Für [Linux][linux], sollten Sie die folgenden Kernelvariablen ändern.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Für [Windows][windows], sollten Sie die Registrierungswerte unten ändern.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Die oben aufgeführten Einstellungen Vergewissern Sie sich ein aktiv beibehalten-Paket nach 2 Minuten (120 Sekunden) Leerlauf gesendet wird, und klicken Sie dann alle 30 Sekunden gesendet. Und wenn 8 von diesen Paketen ein Fehler auftreten, wird die Sitzung gelöscht.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md