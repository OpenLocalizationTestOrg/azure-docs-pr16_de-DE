<properties
   pageTitle="Polymorphismus in Framework zuverlässigen Akteuren | Microsoft Azure"
   description="Erstellen von Hierarchien .NET Schnittstellen und Typen im Framework zuverlässigen Akteuren Wiederverwendung von Funktionen und API Definitionen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorphismus in Framework zuverlässigen Akteuren

Das zuverlässigen Akteuren Framework können Sie mit vielen der Techniken, die Sie, in der objektorientierten Entwurf verwenden möchten Akteuren erstellen. Eine dieser Methoden ist Polymorphie, die kann Typen und Schnittstellen, um weitere erben GRG-Eltern. Vererbung in Framework zuverlässigen Akteuren folgt in der Regel das Modell .NET mit ein paar zusätzliche Einschränkungen.

## <a name="interfaces"></a>Schnittstellen

Das zuverlässigen Akteuren Framework erfordert Sie definieren mindestens eine Schnittstelle vom Akteurtyp Ihrer implementiert werden. Diese Schnittstelle wird verwendet, um eine Proxy-Klasse zu erzeugen, die von Clients kann, zur Kommunikation mit Ihrem Akteuren dar verwendet werden. Schnittstellen können von anderen Schnittstellen erben, solange jeder Benutzeroberfläche, die von einem Akteurtyp implementiert wird und alle übergeordneten schließlich von IActor abgeleitet werden. IActor ist die Plattform defined Basis Benutzeroberfläche für Akteuren dar. Auf diese Weise könnte im klassischen Polymorphismus Beispiel mit Shapes ungefähr wie folgt aussehen:

![Hierarchie der Benutzeroberfläche für Form Akteuren][shapes-interface-hierarchy]


## <a name="types"></a>Datentypen

Sie können auch eine Hierarchie von Akteur-Typen erstellen, die von der Basis Akteur-Klasse abgeleitet werden, die von der Plattform bereitgestellt wird. Im Falle von Shapes, möglicherweise müssen Sie eine Basis `Shape` Typ:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Der Subtypen `Shape` können Methoden aus der Basis überschrieben.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Hinweis Die `ActorService` Attribut auf den Akteurtyp. Dieses Attribut mitgeteilt, zuverlässigen Akteur, dass einen Dienst für hostet Akteuren dieses Typs automatisch erstellt werden soll. In einigen Fällen möchten Sie möglicherweise einen Basistyp zu erstellen, der ausschließlich richtet sich Untertypen Funktionalität Freigabe und nie Beton Akteuren instanziieren verwendet werden. In diesen Fällen sollten Sie die `abstract` Schlüsselwort, um darauf hinzuweisen, dass Sie nie einen Akteur je nach Typ erstellen möchten.


## <a name="next-steps"></a>Nächste Schritte

- Finden Sie unter Bereitstellen Zuverlässigkeit, Skalierbarkeit und konsistente Zustand [wie das Framework zuverlässigen Akteuren die Dienst Fabric-Plattform nutzt](service-fabric-reliable-actors-platform.md) .
- Informationen Sie zu den [Akteur Lebenszyklus](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
