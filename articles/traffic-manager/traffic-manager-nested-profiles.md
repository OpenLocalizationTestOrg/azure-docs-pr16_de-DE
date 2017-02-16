<properties
    pageTitle="Verschachtelte Datenverkehr Manager Profile | Microsoft Azure"
    description="Dieser Artikel erläutert das Feature 'Profile geschachtelt' von Azure Datenverkehr Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Geschachtelte Datenverkehr-Manager-Profilen

Datenverkehr Manager enthält einen Zellbereich Datenverkehr-routing Methoden, mit denen Sie steuern, wie Datenverkehr Manager wählt der Endpunkt Datenverkehr von einzelnen Endbenutzer erhalten soll. Weitere Informationen finden Sie unter [den Datenverkehr Manager Datenverkehr-routing Methoden](traffic-manager-routing-methods.md).

Jeder Datenverkehr-Manager-Profil gibt eine einzelne Datenverkehr-routing Methode an. Es gibt jedoch Szenarien, die erfordern komplexere Datenverkehr routing als das routing von einem einzigen Datenverkehr Manager Profil zur Verfügung gestellt. Sie können den Datenverkehr Manager-Profile, um die Vorteile von mehreren Datenverkehr-routing Methode kombinieren schachteln. Geschachtelte Profilen können Sie das Standardverhalten der Datenverkehr Manager unterstützt größere und komplexere Anwendung Bereitstellungen außer Kraft setzen.

Die folgenden Beispiele veranschaulichen, wie geschachtelte Datenverkehr Manager Profile in verschiedenen Szenarios verwenden.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Beispiel 1: Kombinieren 'Leistung' und 'Weighted' Datenverkehrs-routing

Nehmen Sie an, dass Sie eine Anwendung in den folgenden Azure Regionen bereitgestellt: Westen US, Westen Europa und Ostasien. Datenverkehr Vorgesetzten "Leistung" Datenverkehr-routing Methode verwendet zum Verteilen Datenverkehr an Ihre Region des Benutzers.

![Einzelnes Datenverkehr-Manager-Profil][1]

Nehmen Sie nun an, dass Sie ein Update auf dem Dienst vor stark Weitere paralleles testen möchten. Sie möchten die 'gewichtete' Datenverkehr-routing Methode verwenden, um ein kleiner Prozentsatz der Datenverkehr an Ihre Bereitstellung Test zu leiten. Die Bereitstellung testen entlang der vorhandenen Bereitstellung der Herstellung einrichten in Europa Westen.

Sie können nicht beide 'Weighted' kombinieren und ' Leistung Datenverkehr-routing in einem einzigen Profil. Um dieses Szenario zu unterstützen, erstellen Sie ein Datenverkehr-Manager-Profil, verwenden die beiden Westen Europe Endpunkte und die 'Weighted' Datenverkehr-routing Methode aus. Fügen Sie dieses Profil 'Kind' als Endpunkt in das Profil 'Parent'. Das übergeordnete Profil verwendet die Leistung Datenverkehr-routing Methode weiterhin und enthält die anderen globalen Bereitstellungen als Endpunkte.

Das folgende Diagramm veranschaulicht, in diesem Beispiel:

![Geschachtelte Datenverkehr-Manager-Profilen][2]

In dieser Konfiguration verteilt über die übergeordneten Profil gerichtete Datenverkehr Verkehr auf Regionen Normal. In Europa Westen verteilt das geschachtelte Profil den Datenverkehr in die Endpunkte Herstellung und Test entsprechend der Gewichtung zugeordnet.

Wenn das übergeordnete Profil die "Leistung" Datenverkehr-routing Methode verwendet wird, muss jeder Endpunkt einen Speicherort zugewiesen werden. Die Position zugeordnet ist, wenn Sie den Endpunkt konfigurieren. Wählen Sie Ihre Azure Region der Bereitstellung. Der Azure Bereiche wird die Positionswerte außerhalb der Internet Wartezeit Tabelle. Weitere Informationen finden Sie unter [Datenverkehr Manager "Leistung" Datenverkehr-routing Methode](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Beispiel 2: Endpunkt Überwachung in Profilen geschachtelt

Datenverkehr-Manager überwacht aktiv den Integritätsstatus jedes Service-Endpunkts an. Wenn Sie ein Endpunkt fehlerhaft ist, weist Datenverkehr Manager Benutzer alternativen Endpunkte, um die Verfügbarkeit von Ihrem Dienst beibehalten. Dieses Endpunktverhalten, für die Überwachung und Failover gilt für alle Datenverkehr-routing Methoden. Weitere Informationen finden Sie unter [Den Datenverkehr Manager Endpunkt überwachen](traffic-manager-monitoring.md). Der Endpunkt für die Überwachung funktioniert für geschachtelte Profile unterschiedlich. Mit geschachtelte Profile ausführen nicht das Profil übergeordneten integritätsprüfungen auf der untergeordneten direkt. Stattdessen wird die Integrität des untergeordnetes Element des Profils Endpunkte zum Berechnen des allgemeinen Zustand des Profils untergeordneten verwendet. Diese Informationen im Gesundheitswesen ist von der Profilhierarchie geschachtelte verteilt. Das übergeordnete-Profil dies Systemzustand, um festzustellen, ob für die direkte Verkehr an das Profil untergeordneten aggregiert. Finden Sie im Abschnitt [häufig gestellte Fragen](#faq) in diesem Artikel detaillierte Informationen zur Überwachung des Systemzustands mit geschachtelte Profilen.

Nehmen Sie mit dem vorherigen Beispiel zurückgeben an, dass die Bereitstellung der Herstellung in Europa Westen fehlschlägt. Standardmäßig weist das Profil 'Kind' alle Datenverkehr an die Bereitstellung testen. Wenn die Test-Bereitstellung ebenfalls fehlschlägt, bestimmt das übergeordnete Profil an, dass das Profil untergeordneten Datenverkehr nicht erhalten soll, da alle untergeordneten Endpunkte fehlerhaft sind. Klicken Sie dann verteilt das Profil übergeordneten den Datenverkehr in den anderen Regionen.

![Geschachtelte Profil Failover (Standardverhalten)][3]

Sie können mit dieser Festlegung zufrieden sein. Oder Sie möglicherweise die betreffenden, dass alle Datenverkehr für Westen Europa jetzt an der Bereitstellung testen anstelle einer begrenzten Datenverkehr geht. Unabhängig davon, die Integrität der Bereitstellung testen möchten Sie über in das andere Regionen möglicher Fehler beim die Herstellung Bereitstellung in Europa Westen fehlschlägt. Klicken Sie zum Aktivieren dieser Failover können Sie den Parameter 'MinChildEndpoints' beim Konfigurieren des untergeordneten Profils als Endpunkt im Profil der übergeordneten angeben. Der Parameter bestimmt die minimale Anzahl von verfügbaren Endpunkte im Profil der untergeordneten. Der Standardwert ist "1". In diesem Szenario legen Sie den Wert MinChildEndpoints auf 2 an. Unter diesen Schwellenwert das Profil übergeordneten betrachtet das gesamte untergeordnete Profil nicht verfügbar sein und leitet den Datenverkehr an die anderen Endpunkte.

Die folgende Abbildung zeigt diese Konfiguration:

![Verschachtelte Profil Failover mit 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>Die 'Priorität' Datenverkehr-routing Methode verteilt gesamten Verkehr zu einem einzigen Endpunkt. Es gibt also etwas Zweck einer Einstellung MinChildEndpoints als "1" für ein untergeordnetes Profil.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Beispiel 3: Priorität Failover Regionen in "Leistung" Daten-routing

Das Standardverhalten für die "Leistung" Datenverkehr-routing Methode soll zu viel geladen am nächstgelegenen Endpunkt und bewirken, dass ein cascading Serie von Fehlern zu vermeiden. Wenn Sie ein Endpunkt schlägt fehl, wird gesamten Verkehr, die an diesen Endpunkt geleitet wurden würde gleichmäßig an die anderen Endpunkte auf alle Regionen verteilt.

!["Leistung" Datenverkehr mit standardmäßigen Failover routing][5]

Nehmen Sie jedoch an Sie lieber das Westen Europe Datenverkehr Failover Westen US und den Datenverkehr in anderen Regionen nur direkte, wenn beide Endpunkte nicht verfügbar sind. Sie können diese Lösung mit einem untergeordneten Profil mit den Datenverkehr-routing 'Priorität'-Methode erstellen.

!["Leistung" Datenverkehr mit während Failover routing][6]

Da der Endpunkt Westen Europe höheren Priorität als den Westen US-Endpunkt aufweist, wird an den Endpunkt Westen Europe gesamten Verkehr gesendet, wenn beide Endpunkte online sind. Wenn Europe Westen fehlschlägt, wird der Datenverkehr auf Westen US weitergeleitet. Mit dem geschachtelte Profil wird den Datenverkehr auf Ostasien weitergeleitet, nur, wenn sowohl Westen Europe und Westen US fehl.

Sie können dieses Muster für alle Regionen wiederholen. Ersetzen Sie alle drei Endpunkte im Profil der übergeordneten mit drei untergeordneten Profile jeweils eine Sequenz mit Prioritätsstufe Failover bereitstellen.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Beispiel 4: Steuern des Datenverkehrs-routing zwischen mehreren Endpunkten in derselben Region "Leistung"

Nehmen Sie an die Leistung' Datenverkehr-routing Methode in ein Profil verwendet wird, die mehr als einen Endpunkt in einem bestimmten Bereich verfügt. Standardmäßig ist die Region ein anderes gerichtete Datenverkehr gleichmäßig auf alle verfügbaren Endpunkte in diesem Bereich verteilt.

!["Leistung" Datenverkehr routing in Region Datenverkehr Verteilung (Standardverhalten)][7]

Statt mehrere Endpunkte in Europa Westen hinzufügen, werden diese Endpunkte in einem separaten untergeordneten Profil eingeschlossen. Das Profil der untergeordneten wird das übergeordnete Element als einzige Endpunkt in Europa Westen hinzugefügt. Aktivieren von Priorität-basierten oder gewichtete Datenverkehr routing innerhalb der jeweiligen Region können die Einstellungen im Profil der untergeordneten den Datenverkehr Zufallsvariable mit den Westen Europe steuern.

!["Leistung" Datenverkehr routing benutzerdefinierte in der Region Datenverkehr Wahrscheinlichkeit][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Beispiel 5: Pro Endpunkt Überwachung Einstellungen

Angenommen, Sie verwenden den Datenverkehr Manager störungsfrei migrieren lokalen Datenverkehr von einer Legacy-Website, um eine neue cloudbasierten Version in Azure gehostet wird. Für die legacy-Website, den Sie der Homepage der URI zum Überwachen der Website verwenden möchten. Aber für die neue cloudbasierten Version Sie eine benutzerdefinierte Überwachung Seite implementieren (Pfad ' / monitor.aspx') mit zusätzliche Prüfungen enthält.

![Datenverkehr Manager Endpunkt Überwachung (Standardverhalten)][9]

Die überwachen Einstellungen in den Datenverkehr Manager Profil gelten für alle Endpunkte innerhalb eines einzelnen Profils. Mit geschachtelte Profile verwenden Sie ein Profil der verschiedenen untergeordneten pro Website zum Definieren von verschiedenen Überwachung Einstellungen.

![Datenverkehr Manager Endpunkt mit pro Endpunkt Einstellungen für die Überwachung][10]

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-do-i-configure-nested-profiles"></a>Wie konfigurieren kann ich geschachtelte Profile?

Geschachtelte Datenverkehr Manager Profile können sowohl Ressourcenmanager Azure als auch die klassische Azure REST APIs Azure PowerShell-Cmdlets und Plattformen Azure CLI-Befehle konfiguriert sein. Sie werden auch über das neue Azure-Portal unterstützt. Sie werden nicht in der klassischen Portal unterstützt.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Wie vielen Ebenen schachteln Features Datenverkehr-Manager unterstützt?

Sie können Profile bis zu 10 Ebenen verschachteln. 'Schleifen' sind nicht zulässig.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Können andere Endpunkttypen mit geschachtelte untergeordnete Profile in demselben Datenverkehr Manager Profil werden kombinieren?

Ja. Es gibt keine Einschränkungen auf, wie Sie die Endpunkte des Arten innerhalb eines Profils kombinieren.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Wie gilt Abrechnung Modell für geschachtelt Profile?

Es gibt keine negativen Einfluss der Verwendung von Profiles geschachtelter Preise.

Abrechnung Datenverkehr Manager besteht aus zwei Komponenten: integritätsprüfungen Endpunkt und Millionen von DNS-Abfragen

- Integritätsprüfungen Endpunkt: keine Gebühren für ein Profil untergeordneten, wenn als einen Endpunkt in einem übergeordneten Profil konfiguriert. Überwachung der Endpunkte im Profil der untergeordneten werden wie gewohnt belastet.
- DNS-Abfragen: jeder Abfrage wird nur einmal gezählt. Eine Abfrage in einem übergeordneten-Profil, das einen Endpunkt aus einem Kind-Profil gibt, wird für das übergeordnete Profil nur gezählt.

Einzelheiten hierzu finden Sie unter der [Preisgestaltung Seite Datenverkehr-Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Gibt es eine Auswirkung auf die Leistung für geschachtelte Profile?

Nein. Es gibt keine Auswirkung auf tatsächlich bei Verwendung von verschachtelten Profile die Leistung aus.

Die Namenserver Datenverkehr Manager durchlaufen die Profilhierarchie intern bei der Verarbeitung von einzelnen DNS-Abfrage an. Eine DNS-Abfrage zu einem übergeordneten Profil kann DNS-Einträge mit einen Endpunkt aus einem untergeordneten Profil beantwortet. Ein einzelner CNAME-Eintrag dient, ob Sie ein einzelnes Profil oder geschachtelte Profile verwendet werden. Es ist nicht erforderlich, erstellen einen CNAME-Eintrag für jedes Profil in der Hierarchie ein.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Wie berechnen Datenverkehr-Manager die Integrität des eine geschachtelte Endpunkt in einem übergeordneten Profil?

Das übergeordnete Profil ausführen nicht auf dem untergeordneten integritätsprüfungen direkt. Stattdessen werden die Integrität des untergeordnetes Element des Profils Endpunkte zum Berechnen von des allgemeinen Zustand des Profils untergeordneten verwendet. Diese Informationen weitergegeben, von der Profilhierarchie geschachtelte, um die Integrität des geschachtelte Endpunkts zu bestimmen. Das Profil übergeordneten mithilfe dieser zusammengefasster Gesundheit bestimmt, ob der Datenverkehr auf das untergeordnete weitergeleitet werden kann.

Die folgende Tabelle beschreibt das Verhalten von Datenverkehr Manager Gesundheit für einen Endpunkt geschachtelte überprüft.

|Profil-Monitor untergeordneten status|Übergeordnete Endpunkt Monitor status|Notizen|
|---|---|---|
|Deaktiviert. Das Profil der untergeordneten wurde deaktiviert.|Beendet|Der Status des übergeordneten Endpunkt angehalten nicht deaktiviert. Im Zustand deaktiviert ist reserviert, um anzuzeigen, dass Sie den Endpunkt im übergeordneten Profil deaktiviert haben.|
|Heruntergestuft. Mindestens ein untergeordnetes Element Profil Endpunkt ist in einem Zustand heruntergestuft.| Online: die Anzahl der Online Endpunkte im Profil der untergeordneten ist mindestens den Wert von MinChildEndpoints.<BR>CheckingEndpoint: die Anzahl der Online plus CheckingEndpoint Endpunkte im Profil der untergeordneten ist mindestens den Wert von MinChildEndpoints.<BR>Heruntergestuft: andernfalls.|Datenverkehr wird an einen Endpunkt Status CheckingEndpoint weitergeleitet. Wenn MinChildEndpoints zu hoch festgelegt ist, ist immer der Endpunkt heruntergestuft.|
|Online. Mindestens ein untergeordnetes Element Profil Endpunkt ist Onlinestatus. Kein Endpunkt befindet sich im Status heruntergestuft.|Siehe oben.||
|CheckingEndpoints. Mindestens ein untergeordnetes Element Profil Endpunkt ist 'CheckingEndpoint'. Werden keine Endpunkte 'Online' oder 'Heruntergestuft'|Dasselbe wie oben.||
|Inaktive. Alle untergeordneten Profil Endpunkte sind, entweder deaktiviert oder angehalten, oder dieses Profil sind keine Endpunkte.|Beendet||


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Funktionsweise der Datenverkehr-Manager](traffic-manager-how-traffic-manager-works.md)

Informationen zum [Erstellen eines Profils Datenverkehr-Managers](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

