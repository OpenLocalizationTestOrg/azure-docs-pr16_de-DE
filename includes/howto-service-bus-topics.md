## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Dienstbus Themen und Abonnements?

Unterstützung von Dienstbus Themen und Abonnements einer *Veröffentlichen/Abonnieren* messaging Kommunikationsmodell. Bei Verwendung von Themen und Abonnements, führen Sie die Komponenten einer verteilten Anwendung nicht direkt miteinander kommunizieren; Stattdessen exchange diese Nachrichten über ein Thema, das als Vermittler fungiert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Im Gegensatz zu den Servicebuswarteschlangen, in denen jede Nachricht von einer einzelnen Consumer, verarbeitet wird bieten Themen und Abonnements ein Formulars "eins zu viele" Kommunikationsmethode, unter Verwendung eines Musters veröffentlichen/abonnieren. Es ist möglich, mehrere Abonnements zu einem Thema zu registrieren. Wenn eine Meldung zu einem Thema gesendet wird, wird es dann für jedes Abonnement verfügbar versucht, Ziehpunkt/unabhängig voneinander Prozess.

Ein Abonnement zu einem Thema ähnelt eine virtuelle Warteschlange, die Kopien der Nachrichten empfangen werden, die im Thema gesendet wurden. Sie können optional Filterregeln nach einem Thema auf einer Basis pro Abonnement registrieren, die womit Sie filtern und einschränken, welche Nachrichten zu einem Thema von welche Abonnements Thema empfangen werden können.

Dienstbus Themen und Abonnements ermöglichen Ihnen zu skalieren und eine sehr große Anzahl von Nachrichten bei vielen Benutzern und Applikationen zu verarbeiten.

## <a name="create-a-namespace"></a>Erstellen Sie einen namespace

Um die Verwendung von Dienstbus Themen und Abonnements in Azure beginnen, müssen Sie zunächst einen *Dienstnamespace*erstellen. Ein Namespace stellt einen Bereiche Container zum Adressieren Dienstbus Ressourcen innerhalb Ihrer Anwendung bereit.

So erstellen Sie einen namespace

1. Melden Sie sich bei der [Azure-Portal][]an.

2. Klicken Sie im linken Navigationsbereich des Portals klicken Sie auf **neu**, und dann auf **Enterprise-Integration**, und dann auf **Dienstbus**.

4. Geben Sie im Dialogfeld **Erstellen Namespace** einen Namespacenamen ein. Das System überprüft sofort, um festzustellen, ob der Name verfügbar ist.

5. Nachdem Sie den Namen des Namespaces sicherzustellen verfügbar ist, wählen Sie die Preisgestaltung Ebene (Basic, Standard oder Premium) aus.

7. Wählen Sie im Feld **Abonnement** eines Azure-Abonnements in dem Namespace erstellt.

9. Wählen Sie im Feld **Ressourcengruppe** eine vorhandene Ressourcengruppe, in der der Namespace live, oder erstellen Sie einen neuen, ein.      

8. **Speicherort**auswählen aus die Land / Ihrer Region, in dem der Namespace gehostet werden soll.

    ![Erstellen des namespace][create-namespace]

6. Klicken Sie auf die Schaltfläche **Erstellen** . Das System jetzt Ihren Namespace erstellt und es ermöglicht. Sie müssen möglicherweise warten Sie einige Minuten als die Vorschriften Systemressourcen für Ihr Konto.
 
### <a name="obtain-the-credentials"></a>Stellen Sie die Anmeldeinformationen

1. Klicken Sie in der Liste der Namespaces auf den Namen des neu erstellten Namespaces.
 
3. Klicken Sie in das Blade **Dienstbus Namespace** auf **Freigegebene-Richtlinien**.

4. Klicken Sie in das **freigegebene Access Richtlinien** Blade **RootManageSharedAccessKey**auf.

    ![Verbindung-info][connection-info]

5. In der **Richtlinie: RootManageSharedAccessKey** Blade, klicken Sie auf die Schaltfläche Kopieren neben **Verbindung Zeichenfolge – primär-Taste**, um die Verbindungszeichenfolge in der Zwischenablage zur späteren Verwendung zu kopieren.

    ![Verbindungszeichenfolge][connection-string]

[Azure-portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


