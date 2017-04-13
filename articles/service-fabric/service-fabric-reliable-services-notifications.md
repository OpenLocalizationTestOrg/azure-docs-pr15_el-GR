<properties
   pageTitle="Αξιόπιστη ειδοποιήσεις υπηρεσίες | Microsoft Azure"
   description="Εννοιολογική τεκμηρίωση για τις ειδοποιήσεις αξιόπιστη υπηρεσίες δομής υπηρεσίας"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Αξιόπιστη υπηρεσίες ειδοποιήσεων

Ειδοποιήσεις να επιτρέπεται σε προγράμματα-πελάτες για να παρακολουθείτε τις αλλαγές που γίνονται σε ένα αντικείμενο που βρίσκονται ενδιαφέρει. Δύο τύποι αντικειμένων υποστηρίζει ειδοποιήσεις: *Αξιόπιστη Διαχείριση κατάστασης* και *Αξιόπιστη λεξικό*.

Συνηθισμένων λόγων για τη χρήση ειδοποιήσεων είναι οι εξής:

- Δόμησης επελεύσεως προβολές, όπως δευτερεύων ευρετήρια ή Συγκεντρωτικό φιλτραρισμένες προβολές στη ρεπλίκα κατάστασης. Ένα παράδειγμα είναι ένα ταξινομημένο ευρετήριο όλων των κλειδιών στο λεξικό αξιόπιστη.
- Αποστολή παρακολούθησης δεδομένων, όπως ο αριθμός των χρηστών που έχουν προστεθεί την τελευταία ώρα.

Ειδοποιήσεις ενεργοποιούνται ως μέρος της εφαρμογής λειτουργίες. Λόγω, ειδοποιήσεις πρέπει να αντιμετώπισης εξίσου γρήγορα με πιθανές και σύγχρονη συμβάντα δεν πρέπει να περιλαμβάνουν οποιαδήποτε δαπανηρές.

## <a name="reliable-state-manager-notifications"></a>Αξιόπιστη νομό Διαχείριση ειδοποιήσεων

Διαχείριση αξιόπιστων νομό παρέχει ειδοποιήσεις για τα ακόλουθα:

- Συναλλαγής
    - Υποβολή
- Διαχείριση κατάστασης
    - Εκ νέου δημιουργία
    - Προσθήκη ενός μέλους αξιόπιστη
    - Κατάργηση ενός μέλους αξιόπιστη

Διαχείριση αξιόπιστων κατάσταση παρακολουθεί τις τρέχουσες συναλλαγές inflight. Η αλλαγή μόνο σε κατάσταση συναλλαγής που προκαλεί μια ειδοποίηση για να ενεργοποιείται είναι μια συναλλαγή που δεσμευμένου.

Διαχείριση αξιόπιστων κατάσταση διατηρεί μια συλλογή από αξιόπιστη Πολιτείες όπως αξιόπιστη λεξικό και αξιόπιστη ουρά. Διαχείριση αξιόπιστη κατάσταση ενεργοποιείται ειδοποιήσεις όταν αλλάζει αυτήν τη συλλογή: μια αξιόπιστη κατάσταση έχει προστεθεί ή καταργηθεί ή αναδημιουργείται ολόκληρη τη συλλογή.
Η συλλογή αξιόπιστη Διαχείριση κατάστασης αναδόμηση σε τρεις περιπτώσεις:

- Ανάκτηση: Κατά την εκκίνηση ενός αντιγράφου, το ανακτά στην προηγούμενη κατάσταση από το δίσκο. Στο τέλος της ανάκτησης, χρησιμοποιεί **NotifyStateManagerChangedEventArgs** για να ξεκινήσετε ένα συμβάν που περιέχει το σύνολο των μελών ανακτημένο αξιόπιστη.
- Πλήρες αντίγραφο: πριν από μια ρεπλίκα μπορούν να συμμετάσχουν σε το σύνολο παραμέτρων, πρέπει να είναι ενσωματωμένη. Ορισμένες φορές, αυτό απαιτεί ένα πλήρες αντίγραφο του αξιόπιστη κατάσταση διαχειριστή κατάστασης από την κύρια ρεπλίκα για να εφαρμοστεί στη ρεπλίκα αδράνειας δευτερεύοντα. Διαχείριση αξιόπιστων νομό στη δευτερεύουσα ρεπλίκα χρησιμοποιεί **NotifyStateManagerChangedEventArgs** για να ξεκινήσετε ένα συμβάν που περιέχει το σύνολο των αξιόπιστη μελών που του αποκτήσει από την κύρια ρεπλίκα.
- Επαναφορά: Σε σενάρια αποκατάστασης από καταστροφή, κατάσταση στη ρεπλίκα μπορούν να αποκατασταθούν από ένα αντίγραφο ασφαλείας μέσω **RestoreAsync**. Σε αυτές τις περιπτώσεις, αξιόπιστη Διαχείριση κατάστασης στην κύρια ρεπλίκα χρησιμοποιεί **NotifyStateManagerChangedEventArgs** για να ξεκινήσετε ένα συμβάν που περιέχει το σύνολο των αξιόπιστη μελών που το ανακτηθεί από το αντίγραφο ασφαλείας.

Για να εγγραφείτε για τις ειδοποιήσεις συναλλαγής ή/και κατάσταση Διαχείριση ειδοποιήσεων, πρέπει να καταχωρήσετε με τα συμβάντα **TransactionChanged** ή **StateManagerChanged** στην αξιόπιστη κατάσταση Manager. Κοινές θέσης για την καταχώρηση με αυτά τα προγράμματα χειρισμού συμβάντων είναι η κατασκευή της κατάστασης υπηρεσίας σας. Όταν εγγράφεστε στην κατασκευή, δεν θα χάσετε τυχόν ειδοποίηση που προκαλείται από μια αλλαγή στη διάρκεια της ζωής των **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Το πρόγραμμα χειρισμού συμβάντων **TransactionChanged** χρησιμοποιεί **NotifyTransactionChangedEventArgs** για την παροχή λεπτομερειών σχετικά με το συμβάν. Περιέχει την ιδιότητα ενέργεια (για παράδειγμα, **NotifyTransactionChangedAction.Commit**) που καθορίζει τον τύπο της αλλαγής. Περιέχει επίσης την ιδιότητα συναλλαγών που παρέχει μια αναφορά στην συναλλαγή που άλλαξε.

>[AZURE.NOTE] Σήμερα, τα συμβάντα **TransactionChanged** προκύπτουν μόνο εάν η συναλλαγή δεσμεύεται. Η ενέργεια, στη συνέχεια, είναι ίση με **NotifyTransactionChangedAction.Commit**. Αλλά στο μέλλον, μπορεί να είναι υψωμένο συμβάντων για τους άλλους τύπους των αλλαγών κατάστασης συναλλαγής. Συνιστάται να τον έλεγχο της ενέργειας και την επεξεργασία το συμβάν μόνο εάν είναι μια που αναμένετε.

Ακολουθεί ένα πρόγραμμα χειρισμού συμβάντων **TransactionChanged** παράδειγμα.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Το πρόγραμμα χειρισμού συμβάντων **StateManagerChanged** χρησιμοποιεί **NotifyStateManagerChangedEventArgs** για την παροχή λεπτομερειών σχετικά με το συμβάν.
**NotifyStateManagerChangedEventArgs** έχει δύο υποκατηγορίες: **NotifyStateManagerRebuildEventArgs** και **NotifyStateManagerSingleEntityChangedEventArgs**.
Μπορείτε να χρησιμοποιήσετε την ιδιότητα action στο **NotifyStateManagerChangedEventArgs** για να μετατραπεί **NotifyStateManagerChangedEventArgs** σε τη σωστή υποκατηγορίας:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** και **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Ακολουθεί ένα παράδειγμα **StateManagerChanged** ειδοποίηση χειρισμού.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Αξιόπιστη ειδοποιήσεις λεξικού

Αξιόπιστη λεξικό παρέχει ειδοποιήσεις για τα ακόλουθα:

- Εκ νέου δημιουργία: Κλήση όταν **ReliableDictionary** αποκαταστάθηκε κατάστασή μετά από ένα αντίγραφο ασφαλείας ή τα ανακτημένα ή αντιγράψει τοπική κατάσταση.
- Απαλοιφή: Κλήση όταν η κατάσταση της **ReliableDictionary** έχει καταργηθεί μέσω της μεθόδου **ClearAsync** .
- Προσθέστε: Κλήση όταν ένα στοιχείο έχει προστεθεί **ReliableDictionary**.
- Ενημερωμένη έκδοση του: Κλήση όταν ένα στοιχείο σε **IReliableDictionary** έχει ενημερωθεί.
- Κατάργηση: Κλήση όταν έχει διαγραφεί ένα στοιχείο σε **IReliableDictionary** .

Για να λάβετε ειδοποιήσεις αξιόπιστη λεξικό, πρέπει να καταχωρήσετε με το πρόγραμμα χειρισμού συμβάντων **DictionaryChanged** σε **IReliableDictionary**. Κοινές θέσης για την καταχώρηση με αυτά τα προγράμματα χειρισμού συμβάντων είναι σε το **ReliableStateManager.StateManagerChanged** Προσθήκη ειδοποίησης.
Καταχώρηση όταν **IReliableDictionary** προστίθεται σε **IReliableStateManager** εξασφαλίζει ότι δεν θα χάσετε τις ειδοποιήσεις.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** είναι η μέθοδος δείγμα που καλεί το προηγούμενο παράδειγμα **OnStateManagerChangedHandler** .

Το παραπάνω κώδικα ορίζει το περιβάλλον εργασίας **IReliableNotificationAsyncCallback** , μαζί με **DictionaryChanged**. Επειδή **NotifyDictionaryRebuildEventArgs** περιέχει ένα περιβάλλον εργασίας **IAsyncEnumerable** --ποια πρέπει να ΑΠΑΡΙΘΜΗΣΗ ασύγχρονα--εκ νέου δημιουργία ειδοποιήσεων ενεργοποιούνται μέσω **RebuildNotificationAsyncCallback** αντί για **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Στο προηγούμενο κώδικα, ως μέρος της επεξεργασίας την ειδοποίηση αναδόμηση, πρώτα την κατάσταση διατηρείται συγκεντρωτική είναι απενεργοποιημένο. Επειδή η συλλογή αξιόπιστη αναδόμηση με ένα νέο μέλος, όλες τις ειδοποιήσεις προηγούμενη είναι σημασία.

Το πρόγραμμα χειρισμού συμβάντων **DictionaryChanged** χρησιμοποιεί **NotifyDictionaryChangedEventArgs** για την παροχή λεπτομερειών σχετικά με το συμβάν.
**NotifyDictionaryChangedEventArgs** έχει πέντε υποκατηγορίες. Χρησιμοποιήστε την ιδιότητα ενέργεια σε **NotifyDictionaryChangedEventArgs** για να μετατραπεί **NotifyDictionaryChangedEventArgs** σε τη σωστή υποκατηγορίας:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** και **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Συστάσεις

- *Κάντε* ολοκλήρωση όσο το δυνατόν γρηγορότερα συμβάντα ειδοποίησης.
- *Δεν* εκτελεί οποιαδήποτε δαπανηρές (για παράδειγμα, λειτουργίες εισόδου/εξόδου) ως μέρος του σύγχρονα συμβάντα.
- *Κάντε* επιλέξτε τον τύπο ενέργειας πριν από την επεξεργασία του συμβάντος. Νέοι τύποι ενέργεια μπορεί να προστεθεί στο μέλλον.

Ακολουθούν ορισμένα πράγματα που πρέπει να λάβετε υπόψη:

- Ειδοποιήσεις ενεργοποιούνται ως μέρος του την εκτέλεση μιας λειτουργίας. Για παράδειγμα, μια ειδοποίηση επαναφορά ενεργοποιείται ως το τελευταίο βήμα της λειτουργίας επαναφοράς. Επαναφορά δεν θα ολοκληρωθεί μέχρι το συμβάν ειδοποίησης υποβάλλεται σε επεξεργασία.
- Επειδή ειδοποιήσεις ενεργοποιούνται ως μέρος των λειτουργιών συσχέτισης, προγράμματα-πελάτες βλέπετε μόνο ειδοποιήσεις για τοπικά δεσμευμένη λειτουργίες. Και επειδή λειτουργίες είναι εγγυημένη μόνο τοπικά δεσμευμένη (με άλλα λόγια, συνδεδεμένοι), που μπορεί να ή ενδέχεται να μην είναι δυνατή η αναίρεση στο μέλλον.
- Στη διαδρομή Ακύρωση αναίρεσης, ενεργοποιείται μια ειδοποίηση μία για κάθε εργασία έχουν εφαρμοστεί. Αυτό σημαίνει ότι εάν συναλλαγής Τ1 περιλαμβάνει Create(X), Delete(X) και Create(X), θα λάβετε μια ειδοποίηση για τη δημιουργία του X, μία για τη διαγραφή και μία για τη δημιουργία ξανά, με αυτήν τη σειρά.
- Για συναλλαγές που περιέχουν πολλές εργασίες, οι λειτουργίες εφαρμόζονται με τη σειρά με την οποία ελήφθησαν στην κύρια ρεπλίκα από το χρήστη.
- Ως μέρος της επεξεργασίας false προόδου, ορισμένες λειτουργίες μπορεί να είναι δυνατή η αναίρεση. Ειδοποιήσεις είναι ενεργοποιείται για τέτοιες δραστηριότητες Αναίρεση, επαναφορά της κατάστασης στη ρεπλίκα σε ένα σταθερό σημείο. Μια σημαντική διαφορά των ειδοποιήσεων Αναίρεση είναι ότι έχουν συναθροιστεί συμβάντα που έχουν διπλότυπα κλειδιά. Για παράδειγμα, εάν είναι γίνεται επαναφορά των συναλλαγών Τ1, θα δείτε μια ειδοποίηση μόνο σε Delete(X).

## <a name="next-steps"></a>Επόμενα βήματα

- [Αξιόπιστη συλλογές](service-fabric-work-with-reliable-collections.md)
- [Αξιόπιστη υπηρεσίες γρήγορης εκκίνησης](service-fabric-reliable-services-quick-start.md)
- [Αξιόπιστη υπηρεσίες αντιγράφων ασφαλείας και επαναφοράς (αποκατάσταση)](service-fabric-reliable-services-backup-restore.md)
- [Αναφορά προγραμματιστών για αξιόπιστη συλλογές](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
