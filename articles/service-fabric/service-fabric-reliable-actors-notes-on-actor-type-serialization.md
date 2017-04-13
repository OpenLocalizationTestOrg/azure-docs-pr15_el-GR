<properties
   pageTitle="Αξιόπιστη ηθοποιών σημειώσεις στο παράγοντα πληκτρολογήστε σειριοποίησης | Microsoft Azure"
   description="Περιγράφει βασικές απαιτήσεις για τον ορισμό σειριοποιηθεί κλάσεις που μπορούν να χρησιμοποιηθούν για να ορίσετε μια υπηρεσία ύφασμα αξιόπιστη ηθοποιών μέλη και διασυνδέσεων"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Σημειώσεις στην υπηρεσία ύφασμα αξιόπιστη ηθοποιών πληκτρολογήστε σειριοποίησης


Τα ορίσματα της όλα μεθόδους, τύποι αποτελεσμάτων των εργασιών που επιστρέφονται από κάθε μέθοδο σε ένα περιβάλλον εργασίας παράγοντα, και των αντικειμένων που είναι αποθηκευμένα στη Διαχείριση κατάστασης έναν παράγοντα πρέπει να [Σειριοποιηθεί σύμβασης δεδομένων](https://msdn.microsoft.com/library/ms731923.aspx)... Αυτό ισχύει επίσης για τα ορίσματα από τις μεθόδους που ορίζονται στο [διασυνδέσεων συμβάν παράγοντα](service-fabric-reliable-actors-events.md#actor-events). (Μεθόδους περιβάλλοντος εργασίας συμβάν παράγοντα επιστρέφει πάντα void.)

## <a name="custom-data-types"></a>Τύποι προσαρμοσμένων δεδομένων

Σε αυτό το παράδειγμα, η ακόλουθη διασύνδεση παράγοντα καθορίζει μια μέθοδο που επιστρέφει ένα προσαρμοσμένο τύπο δεδομένων που ονομάζεται `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Το περιβάλλον εργασίας είναι impelemented κατά έναν παράγοντα, που χρησιμοποιεί τη Διαχείριση κατάστασης για την αποθήκευση ενός `VoicemailBox` αντικειμένου:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

Σε αυτό το παράδειγμα, το `VoicemailBox` αντικείμενο είναι σειριοποιημένο όταν:
 - Το αντικείμενο μεταδίδονται μεταξύ μιας παρουσίας παράγοντα και καλών.
 - Το αντικείμενο αποθηκεύεται στη Διαχείριση κατάστασης όπου διατηρηθεί στο δίσκο και αναπαραχθούν σε άλλους κόμβους.
 
Στο πλαίσιο αξιόπιστες παράγοντα χρησιμοποιεί DataContract σειριοποίησης. Επομένως, τα αντικείμενα προσαρμοσμένα δεδομένα και των μελών τους πρέπει να είναι με σχόλια με τα χαρακτηριστικά **DataContract** και **DataMember** , αντίστοιχα

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Επόμενα βήματα
 - [Κύκλος ζωής και απορριμμάτων συλλογής παράγοντα](service-fabric-reliable-actors-lifecycle.md)
 - [Χρονιστές παράγοντα και υπενθυμίσεις](service-fabric-reliable-actors-timers-reminders.md)
 - [Συμβάντα παράγοντα](service-fabric-reliable-actors-events.md)
 - [Reentrancy παράγοντα](service-fabric-reliable-actors-reentrancy.md)
 - [Παράγοντα πολυμορφισμού και μοτίβα προσανατολισμένα σε αντικείμενα σχεδίασης](service-fabric-reliable-actors-polymorphism.md)
 - [Εργαλεία διαγνωστικών παράγοντα και παρακολούθηση των επιδόσεων](service-fabric-reliable-actors-diagnostics.md)
