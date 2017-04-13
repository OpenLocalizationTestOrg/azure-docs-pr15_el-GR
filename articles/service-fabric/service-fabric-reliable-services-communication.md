<properties
   pageTitle="Αξιόπιστη επισκόπηση των υπηρεσιών επικοινωνίας | Microsoft Azure"
   description="Επισκόπηση του μοντέλου επικοινωνίας αξιόπιστων υπηρεσιών, συμπεριλαμβανομένων των ακροατών Άνοιγμα σχετικά με τις υπηρεσίες και επίλυση τελικά σημεία επικοινωνία μεταξύ των υπηρεσιών."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Πώς μπορείτε να χρησιμοποιήσετε APIs αξιόπιστων υπηρεσιών επικοινωνίας

Azure Service ύφασμα ως πλατφόρμα είναι πλήρως είναι ανεξάρτητο σχετικά με την επικοινωνία μεταξύ των υπηρεσιών. Όλα τα πρωτόκολλα και στοίβες είναι αποδεκτή, από UDP στο HTTP. Αυτό εξαρτάται από τον προγραμματιστή της υπηρεσίας για να επιλέξετε πώς θα πρέπει να επικοινωνήσετε υπηρεσίες. Το πλαίσιο εφαρμογή υπηρεσιών αξιόπιστη παρέχει στοίβες ενσωματωμένη επικοινωνίας, καθώς και API που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε τα στοιχεία επικοινωνίας προσαρμοσμένο σας. 

## <a name="set-up-service-communication"></a>Ρύθμιση επικοινωνίας υπηρεσίας

Το API των υπηρεσιών αξιόπιστη χρησιμοποιεί μια απλή διασύνδεση για επικοινωνία με υπηρεσία. Για να ανοίξετε ένα τελικό σημείο της υπηρεσίας, απλώς υλοποίηση αυτή η διασύνδεση:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Μπορείτε να προσθέσετε, στη συνέχεια, υλοποίηση της ακρόασης επικοινωνίας, επιστρέφοντας το σε μια παράκαμψη μέθοδο κλάσης που βασίζεται σε υπηρεσία.

Για τις υπηρεσίες χωρίς κατάσταση:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Για τις υπηρεσίες με κατάσταση:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Και στις δύο περιπτώσεις, μπορείτε να επιστρέψετε μια συλλογή από ακροατών. Αυτό σας επιτρέπει την υπηρεσία για να κάνουν ακρόαση σε πολλές τελικά σημεία, ενδεχομένως με διαφορετικά πρωτόκολλα, με τη χρήση πολλών ακροατών. Για παράδειγμα, μπορεί να έχετε μια ακρόασης HTTP και μια ξεχωριστή παρακολούθηση WebSocket. Κάθε πρόγραμμα ακρόασης λαμβάνει ένα όνομα και, στη συλλογή που προκύπτει από *όνομα: διεύθυνση* ζεύγη αναπαρίσταται ως αντικείμενο JSON όταν ένας υπολογιστής-πελάτης ζητά τις διευθύνσεις ακρόασης για μια παρουσία της υπηρεσίας ή ένα διαμερίσματα.

Σε μια υπηρεσία χωρίς κατάσταση, η αντικατάσταση επιστρέφει μια συλλογή από ServiceInstanceListeners. Μια ServiceInstanceListener περιέχει μια συνάρτηση για να δημιουργήσετε ένα ICommunicationListener και δίνουν ένα όνομα. Για τις υπηρεσίες με κατάσταση, η αντικατάσταση επιστρέφει μια συλλογή από ServiceReplicaListeners. Αυτό συμβαίνει ελαφρώς διαφορετικό από το αντίστοιχο χωρίς κατάσταση, επειδή μια ServiceReplicaListener περιλαμβάνει μια επιλογή για να ανοίξετε ένα ICommunicationListener στη δευτερεύουσα αντίγραφα. Όχι μόνο μπορείτε να χρησιμοποιήσετε πολλά ακροατών επικοινωνίας με μια υπηρεσία, αλλά μπορείτε επίσης να καθορίσετε ποια ακροατών αποδοχή προσκλήσεων σε δευτερεύοντα αντίγραφα και ποιες ακρόαση μόνο στην κύρια αντίγραφα.

Για παράδειγμα, μπορείτε να έχετε μια ServiceRemotingListener που μεταφέρει κλήσεις RPC μόνο στην κύρια αντίγραφα, και μια δεύτερη, προσαρμοσμένη ακρόασης που τίθεται σε Διαβάστε τις αιτήσεις στη δευτερεύουσα αντίγραφα μέσω HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Κατά τη δημιουργία πολλών ακροατών για μια υπηρεσία, κάθε ακρόασης **πρέπει** να έχετε ένα μοναδικό όνομα.

Τέλος, περιγράψτε τα τελικά σημεία που απαιτούνται για την υπηρεσία στο την [υπηρεσία δήλωσης](service-fabric-application-model.md) στην ενότητα τελικά σημεία.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Το ακροατήριο επικοινωνίας να αποκτήσετε πρόσβαση στους πόρους τελικού σημείου που έχει εκχωρηθεί σε αυτήν από το `CodePackageActivationContext` στο το `ServiceContext`. Το ακροατήριο, στη συνέχεια, να ξεκινήσετε ακρόαση για αιτήσεις κατά το άνοιγμα.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Τελικό σημείο πόροι είναι κοινά και για το πακέτο ολόκληρη την υπηρεσία και έχουν εκχωρηθεί από ύφασμα υπηρεσίας όταν ενεργοποιείται το πακέτο υπηρεσίας. Πολλά αντίγραφα υπηρεσία που φιλοξενείται στο ίδιο ServiceHost μπορεί να κάνετε κοινή χρήση στην ίδια θύρα. Αυτό σημαίνει ότι η ακρόαση επικοινωνίας θα πρέπει να υποστηρίζει την κοινή χρήση θύρας. Το συνιστώμενο τρόπο από αυτήν την ενέργεια είναι να χρησιμοποιήσετε τα διαμερίσματα Αναγνωριστικό και Αναγνωριστικό ρεπλίκα/εμφάνισης, κατά τη δημιουργία της διεύθυνσης ακρόαση το ακροατήριο επικοινωνίας.

### <a name="service-address-registration"></a>Καταχώρηση διεύθυνσης υπηρεσίας

Μια υπηρεσία συστήματος που ονομάζεται την *Υπηρεσία ονομάτων* εκτελείται σε ύφασμα υπηρεσία συμπλεγμάτων. Η υπηρεσία ονομασίας είναι ένα μητρώο καταχώρησης για τις υπηρεσίες και τις διευθύνσεις που εκτελεί ακρόαση κάθε παρουσία ή ρεπλίκα της υπηρεσίας σε. Όταν το `OpenAsync` μέθοδο μια `ICommunicationListener` ολοκληρωθεί, την επιστροφή τιμή λαμβάνει καταχωρηθεί στην υπηρεσία ονομασία. Αυτή η τιμή επιστροφής που λαμβάνει δημοσιεύονται στην υπηρεσία ονομασίας είναι μια συμβολοσειρά του οποίου την τιμή μπορεί να είναι οτιδήποτε καθόλου. Αυτή η τιμή συμβολοσειράς είναι τι προγράμματα-πελάτες θα βλέπουν όταν ζητείται μια διεύθυνση για την υπηρεσία από την υπηρεσία ονομασίας.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Υπηρεσία ύφασμα παρέχει API που επιτρέπει στους υπολογιστές-πελάτες και άλλες υπηρεσίες, στη συνέχεια, για να ζητήσετε αυτήν τη διεύθυνση, το όνομα της υπηρεσίας. Αυτό είναι σημαντικό, επειδή η διεύθυνση της υπηρεσίας δεν είναι στατική. Υπηρεσίες μετακινούνται μέσα στο σύμπλεγμα για σκοπούς εξισορρόπηση και διαθεσιμότητα πόρων. Αυτό είναι το μηχανισμό που επιτρέπει στους υπολογιστές-πελάτες για να επιλύσετε τη διεύθυνση ακρόασης για μια υπηρεσία.

> [AZURE.NOTE] Για μια πλήρη Γνωρίστε του πώς να συντάξετε ένα `ICommunicationListener`, ανατρέξτε στο θέμα [υπηρεσίες API Web της υπηρεσίας υφάσματος με OWIN αυτο-φιλοξενίας](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Επικοινωνία με μια υπηρεσία
Το API αξιόπιστη υπηρεσίες παρέχει τις ακόλουθες βιβλιοθήκες για να γράψετε προγράμματα-πελάτες που επικοινωνούν με τις υπηρεσίες.

### <a name="service-endpoint-resolution"></a>Ανάλυση τελικού σημείου υπηρεσίας
Είναι το πρώτο βήμα για την επικοινωνία με μια υπηρεσία για να επιλύσετε μια διεύθυνση τελικού σημείου της διαμερίσματα ή παρουσίας της υπηρεσίας που θέλετε να μιλήσετε. Το `ServicePartitionResolver` κλάση βοηθητικού προγράμματος είναι μια βασική στοιχειώδης που σας βοηθά προγράμματα-πελάτες καθορίζουν το τελικό σημείο μιας υπηρεσίας κατά το χρόνο εκτέλεσης. Στην υπηρεσία ύφασμα ορολογία, η διαδικασία για τον προσδιορισμό του τελικού σημείου υπηρεσίας αναφέρεται ως την *ανάλυση τελικού σημείου υπηρεσίας*.

Για να συνδεθείτε με τις υπηρεσίες μέσα σε ένα σύμπλεγμα, `ServicePartitionResolver` μπορούν να δημιουργηθούν χρησιμοποιώντας τις προεπιλεγμένες ρυθμίσεις. Αυτή είναι η προτεινόμενη χρήση για τις περισσότερες καταστάσεις:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Για να συνδεθείτε με τις υπηρεσίες σε ένα διαφορετικό σύμπλεγμα, μια `ServicePartitionResolver` μπορούν να δημιουργηθούν με ένα σύνολο τελικά σημεία συμπλέγματος πύλης. Σημειώστε ότι τα τελικά σημεία πύλης είναι απλώς διαφορετικό τελικά σημεία για τη σύνδεση με το ίδιο σύμπλεγμα. Για παράδειγμα:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Εναλλακτικά, `ServicePartitionResolver` μπορεί να δοθεί μια συνάρτηση για τη δημιουργία ενός `FabricClient` για να χρησιμοποιήσετε εσωτερικά: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`είναι το αντικείμενο που χρησιμοποιείται για την επικοινωνία με το σύμπλεγμα ύφασμα υπηρεσίας για διάφορες λειτουργίες διαχείρισης στο σύμπλεγμα. Αυτό είναι χρήσιμο όταν θέλετε περισσότερο έλεγχο για τον τρόπο `ServicePartitionResolver` αλληλεπιδρά με το σύμπλεγμά σας. `FabricClient`εκτελεί σε cache εσωτερικά και είναι γενικά καταναλώνοντας για να δημιουργήσετε, οπότε είναι σημαντικό να επαναχρησιμοποιήσετε `FabricClient` παρουσίες όσο είναι δυνατό. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Μια μέθοδο επίλυση, στη συνέχεια, χρησιμοποιείται για να ανακτήσετε τη διεύθυνση της υπηρεσίας ή ένα διαμερίσματα υπηρεσίας για τις υπηρεσίες διαμερίσματα.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Μια διεύθυνση υπηρεσιών μπορούν να επιλυθούν με ευκολία χρησιμοποιώντας μια `ServicePartitionResolver`, αλλά περισσότερη εργασία είναι απαραίτητη για να βεβαιωθείτε ότι η διεύθυνση επιλυθεί μπορεί να χρησιμοποιηθεί σωστά. Το πρόγραμμα-πελάτης θα πρέπει να εντοπίσει εάν η απόπειρα σύνδεσης απέτυχε λόγω μεταβατικές ένα σφάλμα και να επαναληφθεί (π.χ., υπηρεσία μετακινηθεί ή δεν είναι διαθέσιμος προσωρινά), ή ένα μόνιμο σφάλμα (π.χ., έχει διαγραφεί υπηρεσίας ή ο πόρος που ζητήθηκε δεν υπάρχει πλέον). Παρουσιών της υπηρεσίας ή αντίγραφα μπορεί να μετακινηθείτε από κόμβο για να κόμβου οποιαδήποτε στιγμή για πολλούς λόγους. Τη διεύθυνση της υπηρεσίας επιλυθεί μέσω `ServicePartitionResolver` μπορεί να είναι παλιές από το χρόνο σας κώδικα του προγράμματος-πελάτη επιχειρεί να συνδεθεί. Σε αυτήν την περίπτωση ξανά το πρόγραμμα-πελάτης θα πρέπει να επιλύσετε εκ νέου τη διεύθυνση. Παρέχει τις προηγούμενες `ResolvedServicePartition` υποδεικνύει ότι η επίλυση πρέπει να δοκιμάσετε ξανά και όχι απλώς ανάκτηση μια διεύθυνση στο cache.

Συνήθως, τον κώδικα του προγράμματος-πελάτη πρέπει δεν λειτουργεί με το `ServicePartitionResolver` απευθείας. Δημιουργείται και αντανάκλαση, έως επικοινωνίας εργοστάσια προγράμματος-πελάτη στην αξιόπιστη API υπηρεσίες. Το εργοστάσια Χρησιμοποιήστε η επίλυση εσωτερικά για τη δημιουργία ενός αντικειμένου προγράμματος-πελάτη που μπορούν να χρησιμοποιηθούν για την επικοινωνία με τις υπηρεσίες.

### <a name="communication-clients-and-factories"></a>Προγράμματα-πελάτες επικοινωνίας και εργοστάσια

Η βιβλιοθήκη εργοστασίου επικοινωνίας υλοποιεί ένα τυπικό μοτίβο "Επανάληψη" χειρισμού σφαλμάτων που διευκολύνει την επανάληψη συνδέσεις για να επιλυθεί υπηρεσίας τελικά σημεία. Η βιβλιοθήκη εργοστασίου παρέχει το μηχανισμό "Επανάληψη" ενώ παρέχει τα προγράμματα χειρισμού σφαλμάτων.

`ICommunicationClientFactory`Καθορίζει τη βασική διασύνδεση υλοποιηθεί από μια προέλευση του προγράμματος-πελάτη επικοινωνίας που παράγει προγράμματα-πελάτες που να μιλήσετε σε μια υπηρεσία ύφασμα υπηρεσίας. Την εφαρμογή από το CommunicationClientFactory εξαρτάται από στη στοίβα επικοινωνίας που χρησιμοποιούνται από την υπηρεσία ύφασμα υπηρεσίας όπου ο υπολογιστής-πελάτης θέλει να επικοινωνήσουν. Το API αξιόπιστων υπηρεσιών παρέχει μια `CommunicationClientFactoryBase<TCommunicationClient>`. Αυτή η δυνατότητα παρέχει μια βασική υλοποίηση της το `ICommunicationClientFactory` το περιβάλλον εργασίας και εκτελεί εργασίες που είναι κοινά και για όλες τις στοίβες επικοινωνίας. (Αυτές οι εργασίες περιλαμβάνουν χρησιμοποιώντας μια `ServicePartitionResolver` για να προσδιορίσετε το τελικό σημείο εξυπηρέτησης). Προγράμματα-πελάτες υλοποιήστε συνήθως τη συνοπτική κλάση CommunicationClientFactoryBase χειρισμού λογική που αφορά στη στοίβα επικοινωνίας.

Ο υπολογιστής-πελάτης επικοινωνίας απλώς λαμβάνει μια διεύθυνση και χρησιμοποιεί για να συνδεθείτε σε μια υπηρεσία. Το πρόγραμμα-πελάτη μπορούν να χρησιμοποιούν οποιαδήποτε πρωτόκολλο θέλει.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Το πρόγραμμα-πελάτη factory είναι κυρίως υπεύθυνος για τη δημιουργία επικοινωνίας προγράμματα-πελάτες. Για προγράμματα-πελάτες που δεν διατηρούν μια μόνιμη σύνδεση, όπως ένα πρόγραμμα-πελάτη HTTP, το εργοστασίου μόνο πρέπει να δημιουργήσετε και να επιστρέψετε το πρόγραμμα-πελάτη. Άλλα πρωτόκολλα που διατηρείτε μια μόνιμη σύνδεση, όπως ορισμένα δυαδικά πρωτόκολλα, θα πρέπει επίσης να επικυρώνονται από την προέλευση του για να προσδιορίσετε ή όχι η σύνδεση πρέπει να δημιουργηθεί εκ νέου.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Τέλος, ένα πρόγραμμα χειρισμού εξαίρεσης είναι reponsible για τον προσδιορισμό ποια ενέργεια όταν παρουσιάζεται μια εξαίρεση. Εξαιρέσεις κατηγοριοποιούνται σε **retriable** και **μη retriable**. 

 - Απλώς να **μη retriable** εξαιρέσεις εκ νέου δημιουργήθηκε ξανά για να τον καλούντα. 
 - Εξαιρέσεις **Retriable** περαιτέρω κατηγοριοποιούνται σε **μεταβατικές** και **μη μεταβατική**.
  - **Μεταβατικές** εξαιρέσεις είναι εκείνα που απλώς να επαναληφθεί χωρίς να επιλύσετε εκ νέου τη διεύθυνση τελικού σημείου υπηρεσίας. Αυτές θα περιλαμβάνουν μεταβατικές προβλήματα δικτύου ή υπηρεσίας απαντήσεις σφάλματος εκτός από αυτές που υποδεικνύουν τη διεύθυνση τελικού σημείου υπηρεσίας δεν υπάρχει. 
  - **Μη μεταβατική** εξαιρέσεις είναι εκείνα που απαιτούν τη διεύθυνση τελικού σημείου υπηρεσίας να επιλύονται εκ νέου. Αυτά τα συμπεριλάβετε εξαιρέσεις που δηλώνουν δεν ήταν δυνατή η σύνδεση τελικού σημείου υπηρεσίας, που υποδεικνύει ότι η υπηρεσία έχει μετακινηθεί σε διαφορετικό κόμβο. 

Το `TryHandleException` δημιουργεί μια απόφαση για μια δεδομένη εξαίρεση. Εάν αυτό **δεν γνωρίζουν** τι αποφάσεις για να κάνετε σχετικά με την εξαίρεση, αυτό θα πρέπει να επιστρέψει την **τιμή false**. Εάν το **γνωρίζετε** τι απόφαση για να κάνετε, αυτό θα πρέπει να ορίσετε το αποτέλεσμα ανάλογα και να επιστρέψετε **true**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Συνοπτική ματιά
Με μια `ICommunicationClient`, `ICommunicationClientFactory`, και `IExceptionHandler` δημιουργηθεί γύρω από ένα πρωτόκολλο επικοινωνίας, μια `ServicePartitionClient` είναι αναδιπλώνει όλες μαζί και παρέχει ο βρόχος χειρισμού σφαλμάτων και υπηρεσία partition διεύθυνση ανάλυση γύρω από αυτά τα στοιχεία.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Επόμενα βήματα
 - Δείτε ένα παράδειγμα HTTP επικοινωνίας μεταξύ των υπηρεσιών σε ένα [Δείγμα έργου σε GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Κλήσεις απομακρυσμένης διαδικασίας με αξιόπιστη Υπηρεσίες απομακρυσμένης πρόσβασης](service-fabric-reliable-services-communication-remoting.md)

 - [API που χρησιμοποιεί OWIN στις αξιόπιστες υπηρεσίες Web](service-fabric-reliable-services-communication-webapi.md)

 - [WCF επικοινωνίας με χρήση των υπηρεσιών αξιόπιστη](service-fabric-reliable-services-communication-wcf.md)
