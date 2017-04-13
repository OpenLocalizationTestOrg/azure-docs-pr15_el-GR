<properties
   pageTitle="Αξιόπιστη συμβάντα ηθοποιών | Microsoft Azure"
   description="Εισαγωγή συμβάντων για αξιόπιστη ηθοποιών ύφασμα υπηρεσίας."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Συμβάντα παράγοντα
Συμβάντα παράγοντα παρέχει κάποιο τρόπο για την αποστολή ειδοποιήσεων καλύτερο δυνατό από το παράγοντα στα προγράμματα-πελάτες. Παράγοντα συμβάντα έχουν σχεδιαστεί για την επικοινωνία παράγοντα προς υπολογιστή-πελάτη και δεν πρέπει να χρησιμοποιούνται για την επικοινωνία παράγοντα-παράγοντα.

Τα ακόλουθα τμήματα κώδικα δείχνουν πώς μπορείτε να χρησιμοποιήσετε συμβάντα παράγοντα στην εφαρμογή σας.

Ορισμός ένα περιβάλλον εργασίας που περιγράφει τα συμβάντα που δημοσιεύτηκε από το παράγοντα. Αυτή η διασύνδεση πρέπει να προέρχεται από το `IActorEvents` περιβάλλοντος εργασίας. Τα ορίσματα από τις μεθόδους πρέπει να είναι [σύμβασης σειριοποιηθεί δεδομένων](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Οι μέθοδοι πρέπει να επιστρέφουν void, ως συμβάν ειδοποιήσεις είναι ένας τρόπος και βέλτιστες προσπάθειας.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Δηλώστε τα συμβάντα που δημοσιεύτηκε από το παράγοντα στο περιβάλλον εργασίας του παράγοντα.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Από την πλευρά του προγράμματος-πελάτη, εφαρμόζουν το πρόγραμμα χειρισμού συμβάντων.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Στο πρόγραμμα-πελάτη, δημιουργήστε ένα διακομιστή μεσολάβησης για το παράγοντα που δημοσιεύει το συμβάν και εγγραφή σε συμβάντα του.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Σε περίπτωση ανακατευθύνσεις, το παράγοντα ενδέχεται να μην επάνω σε μια διαφορετική διαδικασία ή κόμβο. Ο διακομιστής μεσολάβησης παράγοντα διαχειρίζεται την ενεργή συνδρομές και αυτόματα εκ νέου στο οποίο έχει εγγραφεί τους. Μπορείτε να ελέγχετε το χρονικό διάστημα εκ νέου τη συνδρομή μέσω του `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Για να καταργήσετε τη συνδρομή σας, χρησιμοποιήστε το `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Στην το παράγοντα, δημοσίευση απλώς τα συμβάντα που πραγματοποιούνται. Εάν υπάρχουν οι συνδρομητές στο συμβάν, ο χρόνος εκτέλεσης ηθοποιών θα στέλνετε την ειδοποίηση.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Επόμενα βήματα
 - [Reentrancy παράγοντα](service-fabric-reliable-actors-reentrancy.md)
 - [Εργαλεία διαγνωστικών παράγοντα και παρακολούθηση των επιδόσεων](service-fabric-reliable-actors-diagnostics.md)
 - [Τεκμηρίωση αναφοράς παράγοντα API](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Δείγμα κώδικα](https://github.com/Azure/servicefabric-samples)
