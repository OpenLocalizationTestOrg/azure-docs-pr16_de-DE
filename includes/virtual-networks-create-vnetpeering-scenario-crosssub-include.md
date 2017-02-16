## <a name="peering-across-subscriptions"></a>Über Abonnements Peering

In diesem Szenario erstellen Sie eine peering zwischen zwei VNets zu anderen Abonnements gehören.

![Cross Subszenario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

VNet peering basiert auf Access rollenbasierte Steuerelement (RBAC) für die Autorisierung. Für Cross-Abonnements Szenario müssen Sie zuerst ausreichenden Berechtigungen Benutzern gewähren, in denen die Peeringverbindung erstellen wird:

> [AZURE.NOTE] Wenn derselbe Benutzer über beide Abonnements die Berechtigung verfügt, können Sie in Schritt 1 – 4 unten überspringen.
