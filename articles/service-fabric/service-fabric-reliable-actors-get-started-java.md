<properties
   pageTitle="Γρήγορα αποτελέσματα με την υπηρεσία ύφασμα αξιόπιστη ηθοποιών | Microsoft Azure"
   description="Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε τα βήματα της δημιουργίας, τον εντοπισμό σφαλμάτων και ένα απλό παράγοντα υπηρεσία που βασίζεται με αξιόπιστη ηθοποιών ύφασμα υπηρεσία για την ανάπτυξη."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Γρήγορα αποτελέσματα με το αξιόπιστη ηθοποιών

> [AZURE.SELECTOR]
- [C# στα Windows](service-fabric-reliable-actors-get-started.md)
- [Java σε Linux](service-fabric-reliable-actors-get-started-java.md)

Σε αυτό το άρθρο εξηγεί τα βασικά στοιχεία του Azure Service ύφασμα αξιόπιστη ηθοποιών και σας καθοδηγεί στις τη δημιουργία και την ανάπτυξη μιας απλής εφαρμογής αξιόπιστη παράγοντα σε Java.

## <a name="installation-and-setup"></a>Εγκατάσταση και ρύθμιση
Πριν ξεκινήσετε, βεβαιωθείτε ότι έχετε το περιβάλλον ανάπτυξης ύφασμα υπηρεσίας ρύθμιση στον υπολογιστή σας.
Εάν πρέπει να έχετε ρυθμίσει, μεταβείτε [Γρήγορα αποτελέσματα στο Mac](service-fabric-get-started-mac.md) ή [Γρήγορα αποτελέσματα σε Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Βασικές έννοιες
Για να ξεκινήσετε με αξιόπιστη ηθοποιών, μόνο πρέπει να κατανοήσετε μερικές βασικές έννοιες:

 * **Υπηρεσία παράγοντα**. Αξιόπιστη ηθοποιών βρίσκονται σε αξιόπιστη υπηρεσίες που μπορούν να αναπτυχθούν στην υπηρεσία ύφασμα υποδομής. Παρουσίες παράγοντα ενεργοποιούνται σε μια παρουσία της καθορισμένης υπηρεσίας.
 
 * **Καταχώρηση παράγοντα**. Ως με αξιόπιστη υπηρεσίες, μια υπηρεσία αξιόπιστη παράγοντα πρέπει να έχει εγγραφεί με το περιβάλλον εκτέλεσης υπηρεσίας ύφασμα. Επιπλέον, ο τύπος παράγοντα πρέπει να έχει καταχωρηθεί με το περιβάλλον εκτέλεσης παράγοντα.
 
 * **Περιβάλλον εργασίας παράγοντα**. Το περιβάλλον εργασίας παράγοντα χρησιμοποιείται για να ορίσετε έναν ισχυρό τύπο δημόσια διασύνδεση ενός παράγοντα. Στο μοντέλο ορολογίας αξιόπιστη παράγοντα, το περιβάλλον εργασίας παράγοντα ορίζει τους τύπους των μηνυμάτων που μπορεί να κατανοήσετε το παράγοντα και διαδικασία. Το περιβάλλον εργασίας παράγοντα χρησιμοποιείται από άλλες ηθοποιών και εφαρμογές προγράμματος-πελάτη για "Αποστολή" (ασύγχρονη) μηνυμάτων για να το παράγοντα. Αξιόπιστη ηθοποιών να εφαρμόσετε πολλά περιβάλλοντα εργασίας.
 
 * **ActorProxy τάξης**. Η κλάση ActorProxy χρησιμοποιείται από εφαρμογές προγράμματος-πελάτη για να καλέσετε τις μεθόδους που εκτίθεται μέσω του περιβάλλοντος εργασίας παράγοντα. Η κλάση ActorProxy παρέχει δύο σημαντικά λειτουργίες:
    * Επίλυση ονομάτων: αυτό είναι σε θέση να εντοπίσετε το παράγοντα του συμπλέγματος (εύρεση τον κόμβο του συμπλέγματος εκεί όπου φιλοξενείται).
    * Αποτυχία χειρισμού: μπορούν να επαναλάβετε τη μέθοδο κλήσεων και επίλυση εκ νέου τη θέση παράγοντα μετά από, για παράδειγμα, μια αποτυχία που απαιτεί η παράγοντα για να έχουν μετακινηθεί σε άλλο κόμβο του συμπλέγματος.

Οι ακόλουθοι κανόνες που σχετίζονται με παράγοντα διασυνδέσεων είναι αξίζει η αναφορά:

- Μέθοδοι παράγοντα περιβάλλοντος εργασίας δεν μπορεί να είναι φορτωμένη υπερβολικά.
- Παράγοντα περιβάλλοντος εργασίας δεν πρέπει να περιέχει μεθόδους ανάληψης, ref ή προαιρετικές παραμέτρους.
- Γενικό διασυνδέσεων δεν υποστηρίζονται.

## <a name="create-an-actor-service"></a>Δημιουργία μιας υπηρεσίας παράγοντα
Ξεκινήστε με τη δημιουργία νέας εφαρμογής υπηρεσίας ύφασμα. Η υπηρεσία SDK ύφασμα για Linux περιλαμβάνει μια Yeoman γεννήτρια για την παροχή του ικριώματος για μια εφαρμογή υπηρεσίας υφάσματος με μια υπηρεσία, χωρίς κατάσταση. Ξεκινήστε εκτελώντας το ακόλουθο Yeoman εντολή:

```bash
$ yo azuresfjava
```

Ακολουθήστε τις οδηγίες για να δημιουργήσετε μια **Αξιόπιστη υπηρεσία παράγοντα**. Για αυτό το πρόγραμμα εκμάθησης, δώστε ένα όνομα στην εφαρμογή "HelloWorldActorApplication" και το παράγοντα "HelloWorldActor." Το παρακάτω ικρίωμα θα δημιουργηθούν:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Αξιόπιστη ηθοποιών βασικό μπλοκ δόμησης

Τις βασικές έννοιες που περιγράφεται παραπάνω μετάφραση σε το βασικό μπλοκ δόμησης μιας υπηρεσίας παράγοντα αξιόπιστη.

### <a name="actor-interface"></a>Περιβάλλον εργασίας παράγοντα

Περιέχει τον ορισμό του περιβάλλοντος εργασίας για το παράγοντα. Αυτή η διασύνδεση Καθορίζει τη σύμβαση παράγοντα που χρησιμοποιείται από κοινού από την εφαρμογή παράγοντα και τα προγράμματα-πελάτες καλεί το παράγοντα, ώστε να συνήθως έχει νόημα να ορίσετε σε μια θέση στην οποία είναι ξεχωριστό από την εφαρμογή παράγοντα και μπορεί να είναι κοινόχρηστο από πολλές άλλες υπηρεσίες ή εφαρμογές προγράμματος-πελάτη.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Υπηρεσία παράγοντα 
Περιέχει τον παράγοντα εφαρμογή και τον κωδικό καταχώρησης παράγοντα. Τάξη παράγοντα υλοποιεί το περιβάλλον παράγοντα. Αυτό είναι όπου τον παράγοντα κάνει την εργασία.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Καταχώρηση παράγοντα

Η υπηρεσία παράγοντα πρέπει να έχει εγγραφεί με έναν τύπο υπηρεσίας στο χρόνο εκτέλεσης υπηρεσίας ύφασμα. Με τη σειρά για την υπηρεσία παράγοντα για να εκτελέσετε τις παρουσίες παράγοντα, τον τύπο παράγοντα επίσης πρέπει να έχει εγγραφεί με την υπηρεσία παράγοντα. Το `ActorRuntime` μέθοδος δήλωσης εκτελεί αυτήν την εργασία ηθοποιών.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Έλεγχος προγράμματος-πελάτη

Αυτή είναι μια εφαρμογή προγράμματος-πελάτη απλή δοκιμή μπορείτε να εκτελέσετε ξεχωριστά από την εφαρμογή υπηρεσίας ύφασμα για να δοκιμάσετε την υπηρεσία παράγοντα. Αυτό είναι ένα παράδειγμα της όπου το ActorProxy μπορεί να χρησιμοποιηθεί για να ενεργοποιήσετε και να επικοινωνούν με παρουσίες παράγοντα. Αυτό δεν αναπτύσσονται και με την υπηρεσία.

### <a name="the-application"></a>Η εφαρμογή 

Τέλος, η εφαρμογή πακέτων την υπηρεσία παράγοντα και οποιεσδήποτε άλλες υπηρεσίες που μπορείτε να προσθέσετε στο μέλλον μαζί για ανάπτυξη. Περιέχει τους κατόχους *ApplicationManifest.xml* και θέσης για το πακέτο υπηρεσίας παράγοντα.

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Το Yeoman ικριώματος περιλαμβάνει μια δέσμη ενεργειών gradle για τη δημιουργία της εφαρμογής και πάρτι δέσμες ενεργειών για την ανάπτυξη και καταργήστε-ανάπτυξη της εφαρμογής. Για να εκτελέσετε την εφαρμογή, δημιουργήστε πρώτα την εφαρμογή με gradle:

```bash
$ gradle
```

Αυτό θα δημιουργήσουν ένα πακέτο εφαρμογών υπηρεσίας ύφασμα που μπορούν να αναπτυχθούν χρησιμοποιώντας CLI Azure ύφασμα υπηρεσίας. Η δέσμη ενεργειών install.sh περιέχει τις απαραίτητες Azure CLI εντολές για να αναπτύξετε το πακέτο εφαρμογών. Απλώς εκτελέστε τη δέσμη ενεργειών install.sh για την ανάπτυξη:

```bask
$ ./install.sh
```
