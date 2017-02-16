<properties
   pageTitle="Erste Schritte beim Erstellen eines internen Lastenausgleich in Ressourcenmanager über das Azure-Portal | Microsoft Azure"
   description="Erfahren Sie, wie eine interne Lastenausgleich in Ressourcenmanager über das Azure-Portal erstellen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Erstellen Sie eine interne Lastenausgleich Azure-Portal

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Erste Schritte beim Erstellen eines internen Lastenausgleich mit Azure-portal

Gehen Sie folgendermaßen vor, um eine interne Lastenausgleich vom Azure-Portal zu erstellen.

1. Öffnen Sie einen Browser, navigieren Sie zu der [Azure-Portal](http://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto.
2. Klicken Sie in der oberen linken Seite des Bildschirms auf **neu** > **Networking** > **Lastenausgleich**.
3. Geben Sie einen **Namen** für Ihre Lastenausgleich, in das Blade **Erstellen Lastenausgleich** .
4. Klicken Sie unter **Farbschema** **intern**auf.
5. Klicken Sie auf **virtuellen Netzwerk**, und wählen Sie dann das virtuelle Netzwerk, in dem Sie Lastenausgleich erstellen möchten.

    >[AZURE.NOTE] Wenn Sie das virtuelle Netzwerk, die, das Sie verwenden möchten, nicht angezeigt werden, überprüfen Sie den **Speicherort** Sie verwenden die für den Lastenausgleich, und ändern Sie ihn entsprechend.

6. Klicken Sie auf **Subnetz**, und wählen Sie dann auf das Subnetz, in dem Sie Lastenausgleich erstellen möchten.
7. Klicken Sie auf entweder **dynamische** oder **statische**, je nachdem, ob die IP-Adresse für den Lastenausgleich (statisch) oder nicht korrigiert werden sollen, klicken Sie unter **IP-Adresse Zuordnung**.

    >[AZURE.NOTE] Wenn Sie auswählen, um eine statische IP-Adresse verwenden, müssen Sie eine Adresse für den Lastenausgleich bereitzustellen.

8. Klicken Sie unter **Ressourcengruppe** Geben Sie den Namen einer neuen Ressourcengruppe für den Lastenausgleich, entweder oder klicken Sie auf **vorhandene auswählen** und wählen Sie eine vorhandene Ressourcengruppe aus.
9. Klicken Sie auf **Erstellen**.

## <a name="configure-load-balancing-rules"></a>Konfigurieren von Regeln für den Lastenausgleich

Navigieren Sie nach dem Laden Lastenausgleich erstellen zu der Ressource laden Lastenausgleich konfigurieren.
Sie müssen zuerst ein Ressourcenpool Back-End-Adresse und einen Prüfpunkt konfigurieren, bevor Sie eine Regel für den Lastenausgleich konfigurieren.

### <a name="step-1-configure-a-back-end-pool"></a>Schritt 1: Konfigurieren einer Back-End-Ressourcenpool

1. Klicken Sie auf **Durchsuchen**, im Portal Azure > **Lastenausgleich**, und klicken Sie dann auf den Lastenausgleich, die Sie soeben erstellt haben.
2. Klicken Sie in das Blade **Einstellungen** auf **Back-End-Pools**.
3. Klicken Sie in die **Back-End-Adresspools** Blade auf **Hinzufügen**.
4. Geben Sie einen **Namen** für die Back-End-Pool in das Blade **Hinzufügen Back-End-Ressourcenpool** , und klicken Sie dann auf **OK**.

### <a name="step-2-configure-a-probe"></a>Schritt 2: Konfigurieren von einen Prüfpunkt

1. Klicken Sie auf **Durchsuchen**, im Portal Azure > **Lastenausgleich**, und klicken Sie dann auf den Lastenausgleich, die Sie soeben erstellt haben.
2. Klicken Sie in der Blade- **Einstellungen** auf **Überprüfen**.
3. Klicken Sie in das Blade **untersucht** auf **Hinzufügen**.
4. Geben Sie in das Blade **Prüfpunkt hinzufügen** einen **Namen** für den Prüfpunkt ein.
5. Wählen Sie unter **Protokoll** **HTTP** (für Websites) oder **TCP** (für andere Programme TCP-basierte) aus.
6. Geben Sie unter **Port**den Port für den Prüfpunkt den Zugriff auf ein.
7. Geben Sie den Pfad zum als einen Prüfpunkt verwenden, klicken Sie unter **Pfad** (für nur HTTP Prüfpunkte).
8. Klicken Sie unter **Intervall** geben an, wie häufig die Anwendung Prüfpunkt.
9. Geben Sie unter **fehlerhaften Schwellenwert**ein wie viele Versuche fehlschlagen sollte, bevor die Back-End-virtuellen Computern als fehlerhaft gekennzeichnet ist.
10. Klicken Sie auf **OK** , um Prüfpunkt zu erstellen.

### <a name="step-3-configure-load-balancing-rules"></a>Schritt 3: Konfigurieren von Regeln für den Lastenausgleich

1. Klicken Sie auf **Durchsuchen**, im Portal Azure > **Lastenausgleich**, und klicken Sie dann auf den Lastenausgleich, die Sie soeben erstellt haben.
2. Klicken Sie in das Blade **Einstellungen** auf **den Lastenausgleich Regeln**.
3. Klicken Sie in das Blade **Lastenausgleich Regeln** auf **Hinzufügen**.
4. Geben Sie in das Blade **Hinzufügen laden Lastenausgleich Regel** einen **Namen** für die Regel ein.
5. Wählen Sie unter **Protokoll** **HTTP** (für Websites) oder **TCP** (für andere Programme TCP-basierte) aus.
6. Geben Sie unter **Port**, dass die Clients Port Verbindung Lastenausgleich aus.
7. Geben Sie unter **Back-End-Port**den Port, in die Back-End-Pool verwendet werden soll (normalerweise den Laden Lastenausgleich-Anschluss und die Back-End-Anschluss sind gleich).
8. Wählen Sie unter **Back-End-Pool**den Back-End-Pool, die, dem Sie soeben erstellt haben.
9. Wählen Sie unter **Beibehaltung der Sitzung**wie Sitzungen beibehalten werden soll.
10. Geben Sie unter **im Leerlauf Timeout (Minuten)**das im Leerlauf Timeout ein.
11. Klicken Sie unter **IP-beweglich (direkte Server zurückgegeben)**klicken Sie auf **deaktiviert** oder **aktiviert**.
12. Klicken Sie auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)