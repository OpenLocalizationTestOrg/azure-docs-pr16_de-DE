<properties
   pageTitle="Aktualisieren einen Azure Service Fabric Cluster | Microsoft Azure"
   description="Aktualisieren Sie den Dienst Fabric Code und/oder Konfiguration, die einen Dienst Fabric Cluster, einschließlich Cluster Update Modus Upgrade Zertifikate, Hinzufügen von Anwendungsports, Ausführen von OS-Patches festlegen ausgeführt wird und so weiter. Was rechnen Sie, wenn die Upgrades durchgeführt werden?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Upgrade einer Azure Service Fabric cluster

> [AZURE.SELECTOR]
- [Azure Cluster](service-fabric-cluster-upgrade.md)
- [Eigenständige Cluster](service-fabric-cluster-upgrade-windows-server.md)

Bei allen modernen Systemen ist Erstellen eines Konzepts für bedingten-Taste, um langfristiges Erfolg Ihres Produkts eintritt. Ein Cluster Azure Service Fabric ist eine Ressource, die Sie besitzen, aber teilweise von Microsoft verwaltet werden. Dieser Artikel beschreibt, was automatisch verwaltet wird, und was Sie selbst konfigurieren können.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Steuern der Fabric-Version, die für Ihren Cluster ausgeführt wird

Sie können Ihre Cluster automatische Fabric-Upgrades erhalten, wenn Microsoft eine neue Version veröffentlicht, oder lassen die wählen Sie eine unterstützte Fabric Version Ihrer Cluster sein muss, den gewünschten festlegen.

Aktion durch Festlegen der Cluster-Konfigurations "UpgradeMode" auf dem Portal oder verwenden zum Zeitpunkt der Erstellung oder höher auf einem live Cluster Ressourcenmanager 

>[AZURE.NOTE] Vergewissern Sie sich beibehalten Ihrer Cluster eine unterstützte Fabric-Version immer ausgeführt wird. Wie und wir die Version eine neue Version des Diensts Fabric ankündigen, wird die vorherige Version nach mindestens 60 Tage von diesem Zeitpunkt für das Ende des Supports markiert. die neuen Versionen bekannt gegebenen [auf den Dienst Fabric-Teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/ )sind. Wählen Sie dann ist die neue Version steht. 

14 Tage vor Ablauf der Version, die Ihren Cluster ausgeführt wird, die wird ein Ereignis Gesundheit generiert, die in eine Warnung Integritätsstatus Ihrer Cluster zusammengeführt werden. Cluster bleibt in einem Warnung Zustand, bis Sie auf eine unterstützte Fabric Version aktualisieren.


### <a name="setting-the-upgrade-mode-via-portal"></a>Festlegen des Upgrade Modus über-portal 

Wenn Sie den Cluster erstellen, können Sie Cluster zum automatischen oder manuellen festlegen.

![Create_Manualmode][Create_Manualmode]

Sie können den Cluster automatisch oder manuell, wenn Sie sich in einem Cluster live mit festlegen die Oberfläche verwalten. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Upgrade auf eine neue Version auf einem Cluster, die zum manuellen Modus über Portal festgelegt werden.
 
Upgrade auf eine neue Version, Sie müssen, lediglich wählen Sie die verfügbare Version aus der Dropdownliste aus, und speichern. Das Upgrade Fabric erhält automatisch gestartet. Die Cluster Health Richtlinien (eine Kombination aus Knoten Gesundheit und die Integrität alle Programme, die im Cluster ausgeführten) eingehalten wird während des Upgrades.

Wenn die Cluster Health Richtlinien nicht erfüllt ist, wird das Upgrade rückgängig gemacht werden. Führen Sie einen Bildlauf nach unten dieses Dokument, um weitere Informationen zum diese benutzerdefinierten Gesundheit Richtlinien festlegen. 

Nachdem Sie die Probleme, die das Zurücksetzen geführt haben behoben haben, müssen Sie das Upgrade erneut, initiieren, indem Sie die gleichen Schritte als vor.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Festlegen des Upgrade Modus über eine Vorlage Ressourcenmanager 

Fügen Sie die Konfiguration "UpgradeMode hinzu" auf die Definition der Microsoft.ServiceFabric/clusters Ressourcen, und legen Sie die "ClusterCodeVersion" auf einen der unterstützten Fabric-Versionen, wie unten dargestellt, und Bereitstellen Sie klicken Sie dann auf die Vorlage. Sind die gültigen Werte für "UpgradeMode", "Manuell" oder "Automatisch"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Upgrade auf eine neue Version auf einem Cluster, die über eine Vorlage Ressourcenmanager manuellen Modus festgelegt ist.
 
Wenn der Cluster im manuellen Modus wird, um das upgrade auf eine neue Version, ändern Sie die "ClusterCodeVersion" in eine unterstützte Version und bereitstellen. Die Bereitstellung der Vorlage, springt des Upgrades Fabric wird gestartet, automatisch. Die Cluster Health Richtlinien (eine Kombination aus Knoten Gesundheit und die Integrität alle Programme, die im Cluster ausgeführten) eingehalten wird während des Upgrades.

Wenn die Cluster Health Richtlinien nicht erfüllt ist, wird das Upgrade rückgängig gemacht werden. Führen Sie einen Bildlauf nach unten dieses Dokument, um weitere Informationen zum diese benutzerdefinierten Gesundheit Richtlinien festlegen. 

Nachdem Sie die Probleme, die das Zurücksetzen geführt haben behoben haben, müssen Sie das Upgrade erneut, initiieren, indem Sie die gleichen Schritte als vor.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Liste aller verfügbaren Version für alle Umgebungen für ein Abonnement angegebenen abrufen

Führen Sie den folgenden Befehl aus, und sollte erhalten Sie eine Ausgabe vergleichbar ist.

"SupportExpiryUtc" erfahren Ihre wann eine angegebene Release läuft oder ist abgelaufen. Die neueste Version verfügt nicht über ein gültiges Datum – es hat den Wert "9999-12-31T23:59:59.9999999", der bedeutet lediglich, dass das Ablaufdatum noch nicht festgelegt ist.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Fabric Upgrade Verhalten, wenn der Cluster Upgrade Modus automatische ist

Microsoft behält die Fabric Code und die Konfiguration, die in einer Azure Cluster ausgeführt wird. Wir ausführen automatische überwachten Upgrades der Software auf Basis als erforderlich. Diese Upgrades könnte Code, Konfiguration oder beides. Um sicherzustellen, dass die Anwendung keine Auswirkung oder minimaler Auswirkung, da diese Upgrades aufweist, können wir die Upgrades durchgeführt in die folgenden Phasen:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Phase 1: Ein Upgrade mithilfe von alle Cluster Gesundheit Richtlinien ausgeführt wird

In dieser Phase die Upgrades fortgesetzt werden jeweils ein Upgrade Domäne, und die Programme, die im Cluster ausgeführt wurden weiterhin ohne Ausfallzeit ausgeführt. Die Cluster Health Richtlinien (eine Kombination aus Knoten Gesundheit und die Integrität alle Programme, die im Cluster ausgeführten) eingehalten wird während des Upgrades.

Wenn die Cluster Health Richtlinien nicht erfüllt ist, wird das Upgrade rückgängig gemacht werden. Anschließend wird eine e-Mail-Nachricht an den Besitzer des Abonnements gesendet. Die e-Mail enthält die folgende Informationen:

- Benachrichtigung, dass wir hatten die Anwendung ein Upgrades Cluster zurückzusetzen.
- Vorgeschlagene Maßnahmen, sofern vorhanden.
- Die Anzahl der Tage (n) bis wir Phase 2 ausführen.

Wir versuchen, dasselbe Update wenige mehrmals ausführen für den Fall, dass alle Upgrades Infrastruktur anderen Gründen fehlgeschlagen ist. Nach der n Tage ab dem Zeitpunkt an, die die e-Mail gesendet wurde, fahren Sie wir mit Phase 2 aus.

Wenn die Cluster Health Richtlinien erfüllt ist, wird das Upgrade erfolgreich betrachtet und als abgeschlossen markiert. Dies kann in dieser Phase während der anfänglichen Aktualisierung oder keines der Upgrade führt erneut auftreten. Es gibt keine e-Mail-Bestätigung einer erfolgreichen Ausführung aus. Dies ist zu verhindern, dass Sie zu viele e-Mails – eine e-Mail-Nachricht empfangen als Ausnahme zur normalen angezeigt werden sollen. Wir erwarten, dass die meisten Cluster-Upgrades erfolgreich durchgeführt werden kann, ohne die Verfügbarkeit der Anwendung beeinträchtigen.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Phase 2: Ein Upgrade mithilfe der Dienststatus Standardrichtlinien nur ausgeführt wird

Die Richtlinien Dienststatus in dieser Phase werden so festlegen, dass die Anzahl der Anwendungen, die am Anfang des Upgrades fehlerfrei wurden für die Dauer der Aktualisierung unverändert bleibt. Wie Phase 1 die Phase 2-Upgrades fortgesetzt werden jeweils ein Upgrade Domäne, und die Programme, die im Cluster ausgeführt wurden weiterhin ohne Ausfallzeit ausgeführt. Die Cluster Health Richtlinien (eine Kombination aus Knoten Gesundheit und die Integrität alle Programme, die im Cluster ausgeführten) eingehalten wird für die Dauer des Upgrades.

Wenn die Cluster Health Richtlinien facto nicht erfüllt ist, wird das Upgrade rückgängig gemacht werden. Anschließend wird eine e-Mail-Nachricht an den Besitzer des Abonnements gesendet. Die e-Mail enthält die folgende Informationen:

- Benachrichtigung, dass wir hatten die Anwendung ein Upgrades Cluster zurückzusetzen.
- Vorgeschlagene Maßnahmen, sofern vorhanden.
- Die Anzahl der Tage bis wir Phase 3 auszuführen (n).

Wir versuchen, dasselbe Update wenige mehrmals ausführen für den Fall, dass alle Upgrades Infrastruktur anderen Gründen fehlgeschlagen ist. Eine Erinnerung e-Mail wird ein paar Tage vor n Tage nach oben werden gesendet. Nach der n Tage ab dem Zeitpunkt an, die die e-Mail gesendet wurde, fahren Sie wir mit Phase 3 aus. Die e-Mail-Nachrichten, die wir Ihnen in Phase 2 senden sehr ernst unternommen werden müssen und Maßnahmen müssen geöffnet sein.

Wenn die Cluster Health Richtlinien erfüllt ist, wird das Upgrade erfolgreich betrachtet und als abgeschlossen markiert. Dies kann in dieser Phase während der anfänglichen Aktualisierung oder keines der Upgrade führt erneut auftreten. Es gibt keine e-Mail-Bestätigung einer erfolgreichen Ausführung aus.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Phase 3: Ein Upgrade mithilfe der strengen Gesundheit Richtlinien ausgeführt wird

In dieser Phase Richtlinien werden nach Abschluss des Upgrades statt die Integrität des Applications ausgerichtet sind. Nur wenige Cluster Upgrades einhandeln in dieser Phase. Wenn Sie Ihren Cluster dieser Phase erreicht wird, besteht eine hohe Wahrscheinlichkeit, dass die Anwendung wird fehlerhaften und/oder verlieren Verfügbarkeit.

Ähnlich wie die anderen zwei Phasen, fahren Phase 3-Upgrades ein Upgrade Domäne nacheinander an.

Wenn die Cluster Health Richtlinien nicht erfüllt ist, wird das Upgrade rückgängig gemacht werden. Wir versuchen, dasselbe Update wenige mehrmals ausführen für den Fall, dass alle Upgrades Infrastruktur anderen Gründen fehlgeschlagen ist. Anschließend wird der Cluster angeheftet, damit es nicht mehr Support und/oder Upgrades empfangen werden.

Eine e-Mail-Nachricht mit dieser Informationen wird an den Abonnementbesitzer, zusammen mit den Maßnahmen gesendet. Wir erwarten keine Cluster zu einem Zustand zu verschaffen, wo Phase 3 fehlgeschlagen ist.

Wenn die Cluster Health Richtlinien erfüllt ist, wird das Upgrade erfolgreich betrachtet und als abgeschlossen markiert. Dies kann in dieser Phase während der anfänglichen Aktualisierung oder keines der Upgrade führt erneut auftreten. Es gibt keine e-Mail-Bestätigung einer erfolgreichen Ausführung aus.

## <a name="cluster-configurations-that-you-control"></a>Cluster-Konfigurationen, die Sie steuern

Aktualisieren über die Möglichkeit zum Festlegen des Clusters Modus, hier sind die Konfigurationen, die Sie in einem live Cluster ändern können.

### <a name="certificates"></a>Zertifikate

Sie können neue hinzufügen oder Zertifikate für den Cluster und Client über das Portal einfach löschen. Anhand [dieses Dokuments ausführliche Anweisungen](service-fabric-cluster-security-update-certs-azure.md)

![Screenshot, der Zertifikatfingerabdruck Azure-Portal anzeigt.][CertificateUpgrade]


### <a name="application-ports"></a>Anwendungsports

Sie können die Anwendungsports ändern, durch Ändern der Eigenschaften der Lastenausgleich-Ressourcen, die den Knotentyp zugeordnet sind. Können im Portal, oder Sie können direkt Ressourcenmanager PowerShell verwenden.

Führen Sie folgende Schritte aus, um einen neuen Anschluss auf alle virtuellen Computern in einem Knotentyp zu öffnen:

1. Fügen Sie einen neuen Prüfpunkt an die entsprechenden Lastenausgleich aus.

    Wenn Sie Ihren Cluster mithilfe des Portals bereitgestellt, den Lastenausgleich sind mit der Bezeichnung "Pfd-Namen für die Ressource Gruppe-NodeTypename", einen für jeden Knoten. Da die Namen der laden Lastenausgleich nur innerhalb einer Ressourcengruppe spezifisch sind, empfiehlt es sich, wenn Sie unter einer bestimmten Ressourcengruppe suchen.

    ![Screenshot zeigt an, dass ein Lastenausgleich im Portal einen Prüfpunkt hinzu.][AddingProbes]

2. Hinzufügen einer neuen Regel an den Lastenausgleich an.

    Hinzufügen einer neuen Regel auf dem gleichen Lastenausgleich mithilfe des Prüfpunkts, den Sie im vorherigen Schritt erstellt haben.

    ![Hinzufügen einer neuen Regel auf ein Lastenausgleich im Portal.][AddingLBRules]


### <a name="placement-properties"></a>Eigenschaften der Position

Für die einzelnen Knoten Arten können Sie benutzerdefinierte Platzierungseigenschaften hinzufügen, die Sie in Ihrer Anwendung verwenden möchten. NodeType ist eine Standardeigenschaft, die Sie verwenden können, ohne ihn explizit hinzuzufügen.

>[AZURE.NOTE] Details zur Verwendung der Platzierung Einschränkungen, Knoteneigenschaften und wie Sie diese definieren finden Sie in Abschnitt "Platzierung Einschränkungen und Knoteneigenschaften" im Dienst Fabric Cluster Ressourcenmanager Dokument auf [Ihr Cluster, beschreibt](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="capacity-metrics"></a>Kapazität Kennzahlen

Für die einzelnen Knoten Arten können Sie benutzerdefinierte Kapazität Kennzahlen hinzufügen, die Sie in Ihrer Anwendung zu Laden des Berichts verwenden möchten. Weitere Informationen über die Verwendung der Kapazität Kennzahlen Bericht Laden Sie, schlagen Sie in der Dienst Fabric Cluster Ressourcenmanager Dokumente auf [Ihr Cluster beschreiben](service-fabric-cluster-resource-manager-cluster-description.md) und [Metrik und Laden](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Fabric-Upgradeeinstellungen - Richtlinien Dienststatus

Sie können angeben, dass die benutzerdefinierte Integrität für Fabric Upgrade Richtlinien. Wenn Sie Ihren Cluster auf automatische Fabric Upgrades festgelegt haben, erhalten diese Richtlinien der Phase-1 für die automatische Fabric Aktualisierung angewendet.
Wenn Sie Ihre Cluster für manuelle Fabric Upgrades eingerichtet haben, können klicken Sie dann diesen Richtlinien jedes Mal angewendet werden, wählen Sie eine neue Version des Systems zum Deaktivieren der Fabric Aktualisierung in Ihren Cluster Starten eines auslösen. Wenn Sie nicht die Richtlinien außer Kraft, werden die Standardwerte verwendet.

Sie können die benutzerdefinierte Integritätsrichtlinien angeben oder überprüfen die aktuellen Einstellungen unter dem Blade "Fabric Upgrade", indem Sie die erweiterten Upgradeeinstellungen auswählen. Überprüfen Sie die folgende Abbildung zum. 

![Verwalten von benutzerdefinierten Dienststatus][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Anpassen der Fabric Einstellungen für Ihren cluster

Schlagen Sie in [Service Fabric Cluster Fabric Einstellungen](service-fabric-cluster-fabric-settings.md) auf was und wie Sie sie anpassen können.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS Patches auf den virtuellen Computern, die den Cluster zusammensetzt

Diese Funktion ist als eine automatisierte Funktion für die Zukunft geplant. Aber derzeit Sie Ihre virtuellen Computer patch zuständig sind. Eine diesem virtuellen Computer gleichzeitig wünschen, ist erforderlich, damit Sie nicht mehrere nacheinander belegen.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>OS-Upgrades auf den virtuellen Computern, die den Cluster zusammensetzt

Wenn Sie das Bild OS auf den virtuellen Computern im Cluster aktualisieren müssen, müssen Sie es jeweils einen virtuellen Computer ausführen. Sie sind verantwortlich für dieses Upgrade – es gibt es zurzeit keine Automatisierung für diese.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie einige der [Dienst Fabric Cluster Fabric Einstellungen](service-fabric-cluster-fabric-settings.md) anpassen
- Erfahren Sie, wie Sie [Ihren Cluster ein-und skalieren](service-fabric-cluster-scale-up-down.md)
- Erfahren Sie mehr über [Upgrades der Anwendung](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG