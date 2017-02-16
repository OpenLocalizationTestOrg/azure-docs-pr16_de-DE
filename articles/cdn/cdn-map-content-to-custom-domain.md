<properties
     pageTitle="Zum Zuordnen von Azure Content Delivery Network (CDN) Inhalt zu einer benutzerdefinierten Domäne | Microsoft Azure"
     description="In diesem Thema wird veranschaulicht, wie eine benutzerdefinierte Domäne einen CDN Inhaltstyp zugeordnet."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Zum Zuordnen der benutzerdefinierten Domäne zu Endpunkt Content Delivery Network (CDN)
Sie können eine benutzerdefinierte Domäne an einen Endpunkt CDN zugeordnet werden, um Ihren eigenen Domänennamen in URLs auf zwischengespeicherte Inhalt anstatt Unterdomäne des azureedge.net verwenden.

Es gibt zwei Methoden zum Zuordnen von Ihrer benutzerdefinierten Domäne an einen Endpunkt CDN aus:

1. [Erstellen Sie einen CNAME-Eintrag mit Ihrer domänenregistrierungsstelle, und ordnen Sie Ihre benutzerdefinierte Domäne und Unterdomäne an den Endpunkt CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Ein CNAME-Eintrag ist ein DNS-Feature, das eine Quelldomäne wie maps `www.contosocdn.com` oder `cdn.contoso.com`, um eine Zieldomäne. In diesem Fall ist die Quelldomäne an Ihre benutzerdefinierte Domäne und Unterdomäne (eine Unterdomäne, wie **"www"** oder **Cdn** immer erforderlich ist). Die Zieldomäne ist der Endpunkt CDN.  

    Die Verfahren zum Zuordnen von Ihrer benutzerdefinierten Domäne zu Ihrem Endpunkt CDN kann, jedoch zu Fehlern im kurzzeitig Ausfall für die Domäne, während Sie die Domäne im Portal Azure erfassen möchten.

2. [Hinzufügen eines Schritts zwischen-XT für Registrierung mit **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Wenn Ihre benutzerdefinierte Domäne aktuell eine Anwendung mit einer Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL), die erfordert, dass keine Ausfälle werden unterstützt, können Sie die Unterdomäne Azure **Cdnverify** verwenden, um eine mittlere Registrierungsschritt bereitzustellen, sodass die Benutzer auf Ihre Domäne, während die Zuordnung erfolgt DNS zugreifen können sollen.  

Nachdem Sie Ihre benutzerdefinierte Domäne mithilfe einer der oben beschriebenen Verfahren registriert haben, sollten Sie sicherstellen [, dass die benutzerdefinierte Unterdomäne Ihrer CDN Endpunkt verweist auf](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] Sie müssen einen CNAME-Eintrag für Ihre Domäne an den Endpunkt CDN zuordnen Ihrer domänenregistrierungsstelle erstellen. CNAME-Einträge Zuordnen von bestimmter Unterdomänen wie `www.contoso.com` oder `cdn.contoso.com`. Es ist nicht möglich, eine Domäne aus, z. B. einen CNAME-Eintrag zugeordnet `contoso.com`.
>    
> Eine Unterdomäne kann nur einen CDN Endpunkt zugeordnet werden. Der CNAME-Eintrag, den Sie erstellen werden alle an die Unterdomäne an den angegebenen Endpunkt adressierte Datenverkehr weiterleiten.  Wenn Sie zuordnen, z. B. `www.contoso.com` mit Ihrem Endpunkt CDN dann Sie zuordnen können es anderen Azure Endpunkten, wie etwa einen Endpunkt des Speicher-Konto oder einen Cloud-Service-Endpunkts an. Allerdings können Sie verschiedene Unterdomänen dieselbe Domäne für unterschiedliche Service Endpunkte verwenden. Sie können auch den gleichen CDN Endpunkt verschiedene Unterdomänen zuordnen.
>
> Beachten Sie für Endpunkte **Azure CDN von Verizon** (Standard- und Premium), dass diese **90 Minuten** für benutzerdefinierte Domäne Änderungen an CDN Kante Knoten weitergegeben belegt.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Registrieren eines benutzerdefinierten Domänennamens für einen Endpunkt Azure CDN

1.  Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/).
2.  Klicken Sie auf **Durchsuchen**, klicken Sie dann **CDN Profile**, klicken Sie dann auf das CDN Profil mit den Endpunkt einer benutzerdefinierten Domäne zugeordnet werden soll.  
3.  Klicken Sie in das Blade **CDN Profil** auf den CDN-Endpunkt, mit dem Sie die Unterdomäne zuordnen möchten.
4.  Klicken Sie am oberen Rand der Endpunkt Blade auf die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** .  In das **Hinzufügen einer benutzerdefinierten Domänennamens** Blade sehen Sie den Endpunkt Hostnamen abgeleitet von Ihrer CDN-Endpunkt mithilfe der in einen neuen CNAME-Eintrag zu erstellen. Das Format der Adresse Host Name wird angezeigt, als ** &lt;endPointName angibt >. azureedge.net**.  Sie können diese Hostnamen beim Erstellen des CNAME-Eintrags verwendet kopieren.  
5.  Navigieren Sie zu der Website Ihrer domänenregistrierungsstelle, und suchen Sie im Abschnitt zum Erstellen von DNS-Einträgen. Sie können dies in einem Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**finden.
6.  Suchen Sie den Abschnitt für die Verwaltung von CNAMEs aus. Sie müssen möglicherweise zu einer Seite Erweiterte Einstellungen zu wechseln, und suchen Sie nach der Wörter, CNAME, Alias oder Unterdomänen.
7.  Erstellen Sie einen neuen CNAME-Eintrag, der Ihre ausgewählten Subdomain (beispielsweise **"www"** oder **Cdn**) zugeordnet ist mit dem bereitgestellten in das **Hinzufügen einer benutzerdefinierten Domänennamens** Blade Hostnamen ein.
8.  Kehren Sie zu dem **Hinzufügen einer benutzerdefinierten Domänennamens** Blade zurück, und geben Sie Ihrer benutzerdefinierten Domäne, die Unterdomäne, einschließlich der im Dialogfeld. Geben Sie beispielsweise den Domänennamen im Format `www.contoso.com` oder `cdn.contoso.com`.   

    Azure werden überprüfen Sie, ob der CNAME-Eintrag für den Namen der Domäne vorhanden ist, die Sie eingegeben haben. Wenn Sie der CNAME-Eintrag korrekt ist, wird Ihre benutzerdefinierte Domäne überprüft.  Bei Endpunkten **Azure CDN von Verizon** (Standard- und Premium) kann er bis zu 90 Minuten für benutzerdefinierte Domäne Einstellungen auf alle CDN Kante Knoten jedoch auf Objektebene überschrieben werden.  

    Beachten Sie, dass in einigen Fällen es den CNAME-Eintrag, um die Namenserver im Internet verbreitet dauern kann. Wenn Ihre Domäne nicht sofort überprüft und Sie Ihrer Meinung nach der CNAME-Eintrag korrekt ist, warten Sie einige Minuten, und versuchen Sie es erneut.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Registrieren Sie sich für einen Azure CDN Endpunkt die Unterdomäne temporären Cdnverify mithilfe eine benutzerdefinierte Domäne  

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **Durchsuchen**, klicken Sie dann **CDN Profile**, klicken Sie dann auf das CDN Profil mit den Endpunkt einer benutzerdefinierten Domäne zugeordnet werden soll.  
3. Klicken Sie in das Blade **CDN Profil** auf den CDN-Endpunkt, mit dem Sie die Unterdomäne zuordnen möchten.
4. Klicken Sie am oberen Rand der Endpunkt Blade auf die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** .  In das **Hinzufügen einer benutzerdefinierten Domänennamens** Blade sehen Sie den Endpunkt Hostnamen abgeleitet von Ihrer CDN-Endpunkt mithilfe der in einen neuen CNAME-Eintrag zu erstellen. Das Format der Adresse Host Name wird angezeigt, als ** &lt;endPointName angibt >. azureedge.net**.  Sie können diese Hostnamen beim Erstellen des CNAME-Eintrags verwendet kopieren.
5. Navigieren Sie zu der Website Ihrer domänenregistrierungsstelle, und suchen Sie im Abschnitt zum Erstellen von DNS-Einträgen. Sie können dies in einem Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**finden.
6. Suchen Sie den Abschnitt für die Verwaltung von CNAMEs aus. Sie müssen möglicherweise zu einer Seite Erweiterte Einstellungen zu wechseln, und suchen Sie nach Wörtern **CNAME**, **Alias**oder **Unterdomänen**.
7. Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Alias Unterdomäne, der die Unterdomäne **Cdnverify** enthält. Beispielsweise werden die Unterdomäne, die Sie angeben, in dem Format **cdnverify.www** oder **cdnverify.cdn**. Geben Sie dann den Hostnamen Ihrer CDN-Endpunkt im Format **Cdnverify.&lt; EndPointName angibt >. azureedge.net**.
8. Kehren Sie zu dem **Hinzufügen einer benutzerdefinierten Domänennamens** Blade zurück, und geben Sie Ihrer benutzerdefinierten Domäne, die Unterdomäne, einschließlich der im Dialogfeld. Geben Sie beispielsweise den Domänennamen im Format `www.contoso.com` oder `cdn.contoso.com`. Beachten Sie, dass in diesem Schritt nicht die Unterdomäne mit **Cdnverify**voranstellen müssen.  

    Azure werden überprüfen Sie, ob der CNAME-Eintrag für den Domänennamen Cdnverify vorhanden ist, die Sie eingegeben haben.
9. Zu diesem Zeitpunkt nach Azure Ihrer benutzerdefinierte Domäne überprüft wurde, aber den Datenverkehr an Ihre Domäne noch nicht an Ihrem Endpunkt CDN verschoben. Nachdem so lange warten die benutzerdefinierte Domäne Einstellungen zu den CDN Kante Knoten (90 Minuten für **Azure CDN von Verizon**, 1 und 2 Minuten für **Azure CDN von Akamai**) auf Objektebene überschrieben werden dürfen zurück zu Ihrer DNS-Registrierungsstelle-Website und erstellen einen weiteren CNAME-Eintrag hinzu ordnet, die Ihre Subdomain Ihrer CDN Endpunkt. Geben Sie beispielsweise die Unterdomäne als **"www"** oder **Cdn**und der Hostname als ** &lt;endPointName angibt >. azureedge.net**. Mit diesem Schritt Abschluss die Registrierung Ihrer benutzerdefinierten Domäne.
10. Schließlich können Sie den CNAME-Eintrag, die Sie erstellt haben, verwenden **Cdnverify**, wie nur als temporären Schritt nötig war löschen.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Stellen Sie sicher, dass die benutzerdefinierte Unterdomäne Ihrer CDN Endpunkt verweist auf

- Nachdem Sie die Registrierung Ihrer benutzerdefinierten Domäne abgeschlossen haben, können Sie Inhalte, die zwischengespeichert wird an Ihrer CDN Endpunkt mit der benutzerdefinierten Domäne zugreifen.
Zunächst sicherzustellen Sie, dass Sie öffentliche Inhalte, die am Endpunkt zwischengespeichert werden. Wenn Ihre CDN Endpunkt Speicher-Konto zugeordnet ist, wird das CDN beispielsweise Inhalt in öffentlichen Blob Container zwischengespeichert. Klicken Sie zum Testen der benutzerdefinierten Domäne, stellen Sie sicher, dass Ihre Container zum Zulassen öffentlichen Zugriffs festgelegt ist und mindestens ein Blob enthält.
- Navigieren Sie in Ihrem Browser zu der Adresse des Blob mit der benutzerdefinierten Domäne. Wenn Ihre benutzerdefinierte Domäne wird beispielsweise `cdn.contoso.com`, werden die URL einer zwischengespeicherten Blob ähnlich wie die folgende URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Siehe auch

[So aktivieren Sie das Content Delivery Network (CDN) für Azure](./cdn-create-new-endpoint.md)  
