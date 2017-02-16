1. Melden Sie sich bei der [Azure-Portal][]an.

2. Klicken Sie im linken Navigationsbereich des Portals klicken Sie auf **neu**, und dann auf **Enterprise-Integration**, und dann auf **Dienstbus**.

4. Geben Sie im Dialogfeld **Erstellen Namespace** einen Namespacenamen ein. Das System überprüft sofort, um festzustellen, ob der Name verfügbar ist.

5. Nachdem Sie den Namen des Namespaces sicherzustellen verfügbar ist, wählen Sie die Preisgestaltung Ebene (Basic, Standard oder Premium) aus.

7. Wählen Sie im Feld **Abonnement** eines Azure-Abonnements in dem Namespace erstellt.

9. Wählen Sie im Feld **Ressourcengruppe** eine vorhandene Ressourcengruppe, in der der Namespace live, oder erstellen Sie einen neuen, ein.      

8. **Speicherort**auswählen aus die Land / Ihrer Region, in dem der Namespace gehostet werden soll.

    ![Erstellen des namespace][create-namespace]

6. Klicken Sie auf **Erstellen**. Das System jetzt Ihren Namespace erstellt und es ermöglicht. Sie müssen möglicherweise warten Sie einige Minuten als die Vorschriften Systemressourcen für Ihr Konto.
 
### <a name="obtain-the-management-credentials"></a>Stellen Sie die Anmeldeinformationen für die Verwaltung

1. Klicken Sie in der Liste der Namespaces auf den Namen des neu erstellten Namespaces.
 
3. Klicken Sie in das Blade Namespace auf **Freigegebene-Richtlinien**.

4. Klicken Sie in das **freigegebene Access Richtlinien** Blade **RootManageSharedAccessKey**auf.

    ![Verbindung-info][connection-info]

5. In der **Richtlinie: RootManageSharedAccessKey** Blade, klicken Sie auf die Schaltfläche Kopieren neben **Verbindung Zeichenfolge – primär-Taste**, um die Verbindungszeichenfolge in der Zwischenablage zur späteren Verwendung zu kopieren. Fügen Sie diesen Wert in Editor oder einem anderen temporären Speicherort.

    ![Verbindungszeichenfolge][connection-string]

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure-portal]: https://portal.azure.com