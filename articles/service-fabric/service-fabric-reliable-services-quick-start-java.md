<properties
   pageTitle="Γρήγορα αποτελέσματα με τις υπηρεσίες του αξιόπιστη | Microsoft Azure"
   description="Εισαγωγή στη δημιουργία μιας εφαρμογής Microsoft Azure Service υφάσματος με τις υπηρεσίες χωρίς κατάσταση και με κατάσταση."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Γρήγορα αποτελέσματα με τις υπηρεσίες του αξιόπιστη

> [AZURE.SELECTOR]
- [C# στα Windows](service-fabric-reliable-services-quick-start.md)
- [Java σε Linux](service-fabric-reliable-services-quick-start-java.md)

Σε αυτό το άρθρο εξηγεί τα βασικά στοιχεία του Azure αξιόπιστη υπηρεσίες δομής υπηρεσίας και σας καθοδηγεί στις τη δημιουργία και την ανάπτυξη μιας απλής εφαρμογής υπηρεσίας αξιόπιστη γραμμένο σε Java.

## <a name="installation-and-setup"></a>Εγκατάσταση και ρύθμιση
Πριν ξεκινήσετε, βεβαιωθείτε ότι έχετε το περιβάλλον ανάπτυξης ύφασμα υπηρεσίας ρύθμιση στον υπολογιστή σας.
Εάν πρέπει να έχετε ρυθμίσει, μεταβείτε [Γρήγορα αποτελέσματα στο Mac](service-fabric-get-started-mac.md) ή [Γρήγορα αποτελέσματα σε Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Βασικές έννοιες
Για να ξεκινήσετε με τις υπηρεσίες του αξιόπιστη, μόνο πρέπει να κατανοήσετε μερικές βασικές έννοιες:

 - **Τύπος υπηρεσίας**: αυτό είναι υλοποίηση της υπηρεσίας. Ορίζεται από την κλάση γράφετε που εκτείνεται `StatelessService` και οποιοδήποτε άλλο κώδικα ή εξαρτήσεις που χρησιμοποιούνται σε αυτά, μαζί με ένα όνομα και έναν αριθμό έκδοσης.

 - **Η παρουσία της υπηρεσίας με όνομα**: για να εκτελέσετε την υπηρεσία, δημιουργείτε καθορισμένη παρουσίες των τον τύπο της υπηρεσίας, πολύ όπως δημιουργείτε παρουσίες αντικειμένου από έναν τύπο τάξης. Εμφανίσεις της υπηρεσίας είναι στην πραγματικότητα αντικείμενο παρουσίες του τάξη σας υπηρεσία που συντάσσετε. 

 - **Υπηρεσία κεντρικού υπολογιστή**: οι εμφανίσεις με όνομα υπηρεσίας μπορείτε να δημιουργήσετε πρέπει να εκτελείτε μέσα σε μια υπηρεσία παροχής φιλοξενίας. Ο κεντρικός υπολογιστής υπηρεσίας είναι απλώς μια διαδικασία όπου είναι δυνατό να εκτελεστεί παρουσιών της υπηρεσίας σας.

 - **Καταχώρηση υπηρεσίας**: καταχώρηση συγκεντρώνει όλα τα στοιχεία. Ο τύπος της υπηρεσίας πρέπει να έχει εγγραφεί με το περιβάλλον εκτέλεσης υπηρεσίας ύφασμα σε μια υπηρεσία παροχής φιλοξενίας υπηρεσίας για να επιτρέψετε σε ύφασμα υπηρεσίας για να δημιουργήσετε εμφανίσεις του για να εκτελέσετε.  

## <a name="create-a-stateless-service"></a>Δημιουργία μιας χωρίς κατάσταση υπηρεσίας

Ξεκινήστε με τη δημιουργία νέας εφαρμογής υπηρεσίας ύφασμα. Η υπηρεσία SDK ύφασμα για Linux περιλαμβάνει μια Yeoman γεννήτρια για την παροχή του ικριώματος για μια εφαρμογή υπηρεσίας υφάσματος με μια υπηρεσία, χωρίς κατάσταση. Ξεκινήστε εκτελώντας το ακόλουθο Yeoman εντολή:

```bash
$ yo azuresfjava
```

Ακολουθήστε τις οδηγίες για να δημιουργήσετε μια **Αξιόπιστη χωρίς κατάσταση υπηρεσίας**. Για αυτό το πρόγραμμα εκμάθησης, δώστε ένα όνομα στην εφαρμογή "HelloWorldApplication" και η υπηρεσία "HelloWorld". Το αποτέλεσμα θα περιλαμβάνει καταλόγων για το `HelloWorldApplication` και `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Υλοποίηση της υπηρεσίας

Άνοιγμα **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Αυτή η κλάση καθορίζει τον τύπο της υπηρεσίας και μπορεί να εκτελέσει οποιαδήποτε κώδικα. Το API της υπηρεσίας παρέχει δύο σημεία εισόδου για τον κωδικό:

 - Μια μέθοδο σημείο ανοικτού καταχώρησης, που ονομάζεται `runAsync()`, όπου μπορείτε να ξεκινήσετε εκτέλεση οποιαδήποτε φόρτους εργασίας, συμπεριλαμβανομένων των μεγάλη διάρκεια εκτέλεσης υπολογισμού φόρτους εργασίας.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Ένα σημείο καταχώρησης επικοινωνίας, όπου μπορείτε να συνδέσετε σε στοίβα σας επικοινωνίας της επιλογής. Αυτό είναι όπου μπορείτε να ξεκινήσετε λαμβάνει αιτήσεις από τους χρήστες και άλλες υπηρεσίες.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Σε αυτό το πρόγραμμα εκμάθησης, θα σας εστιάζει στην το `runAsync()` μεθόδου του σημείου εισόδου. Αυτό είναι όπου μπορείτε να αρχίσετε αμέσως εκτέλεση του κώδικα.

### <a name="runasync"></a>RunAsync

Η πλατφόρμα κλήσεις αυτήν τη μέθοδο όταν μια παρουσία μιας υπηρεσίας είναι τοποθετημένα και είστε έτοιμοι να εκτελέσει. Ο κύκλος Άνοιγμα/Κλείσιμο μιας παρουσίας υπηρεσίας μπορεί να προκύψουν πολλές φορές κατά τη διάρκεια ζωής της υπηρεσίας ως σύνολο. Αυτό μπορεί να συμβαίνει για διάφορους λόγους, όπως:

- Το σύστημα μετακινεί το παρουσιών της υπηρεσίας για τον πόρο εξισορρόπηση.
- Τα σφάλματα παρουσιάζονται στον κώδικά σας.
- Η εφαρμογή ή το σύστημα έχει αναβαθμιστεί.
- Το υποκείμενο υλικό παρουσιάζει μια μη διαθεσιμότητα.

Θα γίνεται η διαχείριση αυτό ενορχήστρωσης από ύφασμα υπηρεσίας για να διατηρήσετε την υπηρεσία ιδιαίτερα διαθέσιμες και ισορροπημένες σωστά.

#### <a name="cancellation"></a>Ακύρωσης

Είναι απαραίτητο που σας κώδικα στο `runAsync()` να διακόψετε την εκτέλεση όταν κοινοποίηση από ύφασμα υπηρεσίας. Το `CompletableFuture` επιστρέφονται από `runAsync()` ακυρώνεται όταν ύφασμα υπηρεσία απαιτεί την υπηρεσία για να διακόψετε την εκτέλεση. Το παρακάτω παράδειγμα δείχνει τον τρόπο χειρισμού ένα συμβάν ακύρωσης: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Καταχώρηση υπηρεσίας

Τύποι υπηρεσία πρέπει να έχει εγγραφεί με το περιβάλλον εκτέλεσης υπηρεσίας ύφασμα. Ο τύπος υπηρεσίας έχει οριστεί στο το `ServiceManifest.xml` και τάξη σας υπηρεσία που υλοποιεί `StatelessService`. Καταχώρηση υπηρεσίας πραγματοποιείται με τη διαδικασία το κύριο σημείο εισόδου. Σε αυτό το παράδειγμα, είναι η διαδικασία το κύριο σημείο εισόδου `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Το Yeoman ικριώματος περιλαμβάνει μια δέσμη ενεργειών gradle για τη δημιουργία της εφαρμογής και πάρτι δέσμες ενεργειών για την ανάπτυξη και καταργήστε-ανάπτυξη της εφαρμογής. Για να εκτελέσετε την εφαρμογή, δημιουργήστε πρώτα την εφαρμογή με gradle:

```bash
$ gradle
```

Αυτό θα δημιουργήσουν ένα πακέτο εφαρμογών υπηρεσίας ύφασμα που μπορούν να αναπτυχθούν χρησιμοποιώντας CLI Azure ύφασμα υπηρεσίας. Η δέσμη ενεργειών install.sh περιέχει τις απαραίτητες Azure CLI εντολές για να αναπτύξετε το πακέτο εφαρμογών. Απλώς εκτελέστε τη δέσμη ενεργειών install.sh για την ανάπτυξη:

```bask
$ ./install.sh
```
