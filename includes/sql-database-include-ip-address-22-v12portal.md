
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. Melden Sie sich das [Azure-Portal](https://portal.azure.com/) unter http://portal.azure.com/.

2. Klicken Sie im linken Banner auf **Alle durchsuchen**. Das Blade **Durchsuchen** wird angezeigt.

3. Führen Sie einen Bildlauf aus, und klicken Sie auf **SQL Server**. Das **SQL Server** -Blade angezeigt wird.

    ![Suchen Sie Ihre Azure SQL-Datenbankserver im Portal][b21-FindServerInPortal]

4. Klicken Sie auf das Steuerelement minimieren in der früheren Blade **Navigieren Sie** zur Vereinfachung.

5. Starten Sie in das Filtertextfeld Geben Sie den Namen des Servers ein. Die Zeile wird angezeigt.

6. Klicken Sie auf die Zeile mit dem Server. Eine Blade für Ihren Server wird angezeigt.

7. Klicken Sie auf der Serverblade auf **Einstellungen**. Das Blade **Einstellungen** wird angezeigt.

8. Klicken Sie auf **Firewall**. Das Blade **Firewall-Einstellungen** angezeigt wird.

    ![Klicken Sie auf Einstellungen > Firewall][b31-SettingsFirewallNavig]

9. Klicken Sie auf **Hinzufügen Client-IP-**. Geben Sie einen Namen für die neue Regel in das erste Textfeld.

10. Geben Sie die Werte der Höchst- und IP-Adresse für den Bereich, den Sie aktivieren möchten.
    - Es kann low-Value Ende mit **.0** und die hoch **.255**haben, können sehr praktisch sein.

    ![Hinzufügen eines IP-Adressbereichs dürfen][b41-AddRange]

11. Klicken Sie auf **Speichern**.



<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
