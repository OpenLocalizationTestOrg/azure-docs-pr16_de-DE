<properties 
pageTitle="Bereitstellen von S/4 HANA oder BW/4 HANA eine Azure-virtuellen Computers | Microsoft Azure" 
description="Bereitstellen von S/4 HANA oder BW/4 HANA eine Azure-virtuellen Computers" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>Bereitstellen von S/4 HANA oder BW/4 HANA auf Microsoft Azure 

Dieser Artikel beschreibt, wie S/4 HANA auf Microsoft Azure über SAP-Cloud Einheit Bibliothek 3.0 bereitgestellt.
Die Screenshots zeigen den Prozess Schritt für Schritt. Bereitstellen von anderen HANA SAP-basierten Lösungen, wie BW/4 HANA Prozess im Hinblick auf die gleiche Weise funktioniert. Eine muss nur eine andere Lösung auswählen.

Zunächst SAP-Cloud Einheit (SAP-CAL) finden Sie [hier](https://cal.sap.com/). Es gibt ein Blog von SAP zu den neuen [SAP-Cloud Einheit Bibliothek 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)aus. 


Die folgenden Screenshots zeigen schrittweise so S/4 HANA auf Microsoft Azure bereitstellen. Der Prozess funktioniert die gleiche Weise wie für andere Lösungen LikeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Das erste Bild zeigt alle SAP-CAL HANA-basierten Lösungen, die auf Microsoft Azure verfügbar sind.
Exemplarily der "SAP S/4 HANA lokale Edition" (Lösung klicken Sie unten auf den Screenshot) zu wechseln und den Entscheidungsprozess ausgewählt wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Ein neues SAP-CAL Konto muss zuerst erstellt werden. Aktuell sind zwei Optionen für Azure - standard Azure und Azure auf China Festland, die von Partner 21Vianet betrieben wird.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Verfügt über eine die Azure-Abonnement-ID eingeben, die im Azure-Portal - auch finden Sie unter weiter unten Bezugsarten gefunden werden können. Danach muss ein Zertifikat Azure Management heruntergeladen werden.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

In der neuen Azure sucht Portal eine das Element "Abonnements" auf der linken Seite. Klicken Sie auf, um alle aktiven Abonnements für Ihre Benutzer anzeigen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Auswählen eines der Abonnements, und klicken Sie dann auf auswählen, dass die "Verwaltung von Zertifikaten", die es wird erläutert, ist ein neues Konzept "Dienst Hauptbenutzer" für das neue Ressourcenmanager Azure-Modell verwenden.
SAP-CAL ist nicht für dieses neue Modell noch angepasst und ist immer noch "klassisch" und im früheren Azure-Portal für die Arbeit mit der Verwaltung von Zertifikaten erforderlich.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Hier eine das Azure-Portal unter seinem früheren angezeigt. Der Upload eines Zertifikats Management bietet SAP-CAL die Berechtigungen zum Erstellen von virtuellen Computern innerhalb einer Kundenabonnement. Klicken Sie unter "ABONNEMENTS" finde Registerkarte eine die Abonnement-ID, die im Portal SAP-CAL eingegeben werden muss.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Klicken Sie auf der zweiten Registerkarte kann dann das Zertifikat Management hochladen, das vor dem von SAP-CAL heruntergeladen wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Ein kleines Dialogfeld eingeblendet wird, um die heruntergeladene Zertifikatsdatei auszuwählen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Nachdem das Zertifikat hochgeladen wurde die Verbindung zwischen SAP-CAL und den Kunden Azure-Abonnement innerhalb der SAP-CAl getestet werden kann. Eine kleines Nachricht daraufhin gestartet weist, dass die Verbindung gültig ist.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Nach dem Einrichten eines Kontos verfügt über eine Auswahl eine Lösung, die bereitgestellt werden soll, und erstellen Sie eine Instanz aus.
Mit dem Modus "basic" ist es wirklich einfach. Geben Sie einen Instanznamen, wählen Sie eine Azure Region und definieren Sie das master-Kennwort für die Lösung.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Nach einiger Zeit je nach Größe und Komplexität der Lösung (eine Schätzung wird von SAP-CAL angegeben), wird es als "aktiv" und zur Verwendung bereit angezeigt. Es ist sehr einfach.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Einige Details der Lösung eine sehen welche Art von virtuellen Computern bereitgestellt wurden. In diesem Fall wurden drei Azure virtuellen Computern unterschiedliche Größen und Zweck erstellt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Klicken Sie im Portal Azure den virtuellen Computern finden Sie beginnend mit der gleichen Instanznamen, der im SAP-CAL angegeben wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Jetzt ist es möglich, mit der Lösung über die Schaltfläche verbinden im Portal SAP-CAL verbinden. Das Dialogfeld etwas enthält einen Link zu einer Benutzerhandbuch, das die standardmäßigen Anmeldeinformationen für die Arbeit mit der Lösung beschrieben.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Eine weitere Möglichkeit besteht darin Desktopclient virtuellen Windows-Computer anmelden und beispielsweise die vorkonfiguriertes SAP-Benutzeroberfläche starten.







