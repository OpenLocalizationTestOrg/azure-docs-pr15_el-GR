<properties 
    pageTitle="Πρόγραμμα εκμάθησης μεταγωγής Bus υπηρεσίας | Microsoft Azure"
    description="Δόμηση ένα πρόγραμμα-πελάτη Bus υπηρεσίας εφαρμογής και υπηρεσία χρησιμοποιώντας μετάδοση Bus υπηρεσίας."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Πρόγραμμα εκμάθησης μεταγωγής Bus υπηρεσίας

Αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να δημιουργήσετε μια απλή εφαρμογή προγράμματος-πελάτη Bus υπηρεσία και υπηρεσία χρησιμοποιώντας τις δυνατότητες "μετάδοση" Bus υπηρεσίας. Για ένα πρόγραμμα εκμάθησης αντίστοιχο που χρησιμοποιεί υπηρεσία Bus [όπου μεσολαβούν μηνυμάτων](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging), ανατρέξτε στο θέμα της [Υπηρεσίας Bus όπου μεσολαβούν μηνυμάτων .NET πρόγραμμα εκμάθησης](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Εργασία σε αυτό το πρόγραμμα εκμάθησης σάς παρέχει την κατανόηση των βημάτων που απαιτούνται για να δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη και της υπηρεσίας εξυπηρέτησης Bus. Όπως και τα αντίστοιχα WCF, μια υπηρεσία είναι μια δομή που εκθέτει τα τελικά σημεία ενός ή περισσότερων, κάθε μία από τις οποίες εκθέτει μία ή περισσότερες λειτουργίες υπηρεσίας. Το τελικό σημείο μιας υπηρεσίας καθορίζει μια διεύθυνση όπου μπορείτε να βρείτε την υπηρεσία, μια σύνδεση που περιέχει τις πληροφορίες που πρέπει να επικοινωνεί με ένα πρόγραμμα-πελάτη με την υπηρεσία και ένα συμβόλαιο που ορίζει τη λειτουργικότητα που παρέχεται από την υπηρεσία σε υπολογιστές-πελάτες του. Η κύρια διαφορά μεταξύ ενός WCF και μιας υπηρεσίας Bus υπηρεσίας είναι ότι το τελικό σημείο εκτίθεται στο cloud αντί τοπικά στον υπολογιστή σας.

Αφού εργάζεστε έως την ακολουθία θεμάτων σε αυτό το πρόγραμμα εκμάθησης, θα έχετε μια υπηρεσία που εκτελείται και ένα πρόγραμμα-πελάτη που μπορεί να ενεργοποιήσει τις λειτουργίες της υπηρεσίας. Το πρώτο θέμα περιγράφει πώς μπορείτε να ρυθμίσετε ένα λογαριασμό. Τα επόμενα βήματα περιγράφουν πώς μπορείτε να ορίσετε μια υπηρεσία που χρησιμοποιεί ένα συμβόλαιο, τον τρόπο για την υλοποίηση της υπηρεσίας και πώς μπορείτε να ρυθμίσετε την υπηρεσία κώδικα. Αυτά επίσης περιγράφουν πώς μπορείτε να φιλοξενήσετε και να εκτελέσετε την υπηρεσία. Η υπηρεσία που δημιουργείται είναι αυτο-φιλοξενούμενη και εκτέλεση του προγράμματος-πελάτη και η υπηρεσία στον ίδιο υπολογιστή. Μπορείτε να ρυθμίσετε την υπηρεσία, χρησιμοποιώντας το κώδικα ή ένα αρχείο ρύθμισης παραμέτρων.

Το τελικό τρία βήματα περιγράφουν τον τρόπο για να δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη, ρύθμιση παραμέτρων της εφαρμογής υπολογιστή-πελάτη, και δημιουργήστε και χρησιμοποιήστε ένα πρόγραμμα-πελάτη που μπορούν να έχουν πρόσβαση τη λειτουργικότητα του κεντρικού υπολογιστή.

Όλα τα θέματα σε αυτήν την ενότητα λαμβάνεται ως δεδομένο ότι χρησιμοποιείτε Visual Studio ως το περιβάλλον ανάπτυξης.

## <a name="sign-up-for-an-account"></a>Εγγραφείτε για ένα λογαριασμό

Το πρώτο βήμα είναι να δημιουργήσετε ένα χώρο ονομάτων και για να αποκτήσετε έναν αριθμό-κλειδί θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας). Ένας χώρος ονομάτων παρέχει όριο μιας εφαρμογής για κάθε εφαρμογή εκτίθεται μέσω Bus υπηρεσίας. Έναν αριθμό-κλειδί συσχετισμών Ασφαλείας δημιουργείται αυτόματα από το σύστημα όταν δημιουργείται ένα χώρο ονομάτων υπηρεσίας. Ο συνδυασμός χώρος ονομάτων υπηρεσίας και κλειδί συσχετισμών Ασφαλείας παρέχει τα διαπιστευτήρια για Bus Service για τον έλεγχο ταυτότητας πρόσβαση σε μια εφαρμογή του.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Ορισμός ένα συμβόλαιο υπηρεσία WCF για χρήση με Bus υπηρεσίας

Η υπηρεσία σύμβαση καθορίζει ποιες λειτουργίες υποστηρίζει την υπηρεσία (το Web υπηρεσίας ορολογία για μεθόδους ή συναρτήσεις). Συμβάσεις δημιουργούνται καθορίζοντας ένα περιβάλλον εργασίας C++, C# ή Visual Basic. Κάθε μέθοδο στο περιβάλλον εργασίας αντιστοιχεί σε μια συγκεκριμένη λειτουργία τεχνικής υποστήριξης. Κάθε διασύνδεση πρέπει να έχει το χαρακτηριστικό [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) εφαρμοσμένη και κάθε λειτουργία πρέπει να έχει το χαρακτηριστικό [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) εφαρμοσμένη. Εάν μια μέθοδο σε ένα περιβάλλον εργασίας που έχει το χαρακτηριστικό [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) δεν έχει το χαρακτηριστικό [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) , δεν εκτίθεται αυτή τη μέθοδο. Ο κωδικός για αυτές τις εργασίες παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία. Για μεγαλύτερη συζήτησης των συμβάσεων και των υπηρεσιών, ανατρέξτε στο θέμα [Σχεδίαση και υλοποίηση υπηρεσίες](https://msdn.microsoft.com/library/ms729746.aspx) στην τεκμηρίωση WCF.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Για να δημιουργήσετε ένα συμβόλαιο υπηρεσίας Bus με ένα περιβάλλον εργασίας

1. Ανοίξτε το Visual Studio ως διαχειριστής κάνοντας δεξί κλικ το πρόγραμμα στο μενού **Έναρξη** και επιλέξτε **Εκτέλεση ως διαχειριστής**.

2. Δημιουργήστε ένα νέο έργο εφαρμογής κονσόλας. Κάντε κλικ στο μενού **αρχείο** και επιλέξτε **Δημιουργία**και, στη συνέχεια, κάντε κλικ στο **έργο**. Στο παράθυρο διαλόγου **Νέο έργο** , κάντε κλικ στην επιλογή **Visual C#** (εάν **Visual C#** δεν εμφανίζεται, αναζητήστε στην περιοχή **Άλλες γλώσσες**). Κάντε κλικ στο πρότυπο **Εφαρμογής κονσόλας** και ονομάστε το **EchoService**. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

    ![][2]

3. Εγκαταστήστε το πακέτο NuGet Bus υπηρεσίας. Αυτό το πακέτο προσθέτει αυτόματα αναφορές για τις βιβλιοθήκες Bus υπηρεσίας, καθώς και το WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) είναι ο χώρος ονομάτων που σας επιτρέπει να πρόσβαση μέσω προγραμματισμού τις βασικές δυνατότητες του WCF. Υπηρεσία Bus χρησιμοποιεί πολλά από τα αντικείμενα και χαρακτηριστικά του WCF για να ορίσετε συμβόλαια.

    Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ τη λύση και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet για τη λύση**. Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus`. Βεβαιωθείτε ότι είναι επιλεγμένο το όνομα του έργου στο πλαίσιο **εκδόσεις** . Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης.

    ![][3]

3. Στην Εξερεύνηση λύσεων, κάντε διπλό κλικ στο αρχείο Program.cs για να το ανοίξετε στο πρόγραμμα επεξεργασίας, εάν δεν είναι ήδη ανοιχτό.

4. Προσθέστε τα ακόλουθα χρήση προτάσεων στο επάνω μέρος του αρχείου:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Αλλάξτε το όνομα χώρου ονομάτων από το προεπιλεγμένο όνομα του **EchoService** να **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί C# χώρου ονομάτων **Microsoft.ServiceBus.Samples**, δηλαδή το χώρο ονομάτων του τύπου σύμβασης διαχειριζόμενων που χρησιμοποιείται στο αρχείο ρύθμισης παραμέτρων στο βήμα [Ρύθμιση παραμέτρων του προγράμματος-πελάτη WCF](#configure-the-wcf-client) . Μπορείτε να καθορίσετε οποιοδήποτε πεδίο ονόματος που θέλετε, όταν δημιουργείτε αυτό το δείγμα; Ωστόσο, το πρόγραμμα εκμάθησης δεν θα λειτουργεί εκτός αν, στη συνέχεια, τροποποιήστε τα πεδία ονομάτων της σύμβασης και υπηρεσίας ως εκ τούτου, στο αρχείο παραμέτρων της εφαρμογής. Ο χώρος ονομάτων που καθορίζονται στο αρχείο App.config πρέπει να είναι ίδια με το χώρο ονομάτων που καθορίζονται στα αρχεία C#.

1. Αμέσως μετά το `Microsoft.ServiceBus.Samples` δήλωση χώρου ονομάτων, αλλά μέσα στο χώρο ονομάτων, να καθορίσετε ένα νέο περιβάλλον εργασίας με το όνομα `IEchoContract` και να εφαρμόσετε το `ServiceContractAttribute` χαρακτηριστικών για το περιβάλλον εργασίας με μια τιμή χώρο ονομάτων **http://samples.microsoft.com/ServiceModel/Relay/**. Η τιμή του χώρου ονομάτων διαφέρει από το χώρο ονομάτων που χρησιμοποιείτε σε όλο το εύρος του κώδικα. Αντί για αυτό, η τιμή του χώρου ονομάτων χρησιμοποιείται ως ένα μοναδικό αναγνωριστικό για αυτήν τη σύμβαση. Καθορίζει το χώρο ονομάτων ρητά αποτρέπει την προεπιλεγμένη τιμή χώρος ονομάτων προστίθενται στο όνομα της σύμβασης.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Συνήθως, ο χώρος ονομάτων σύμβασης υπηρεσίας περιέχει ένα συνδυασμό ονομασίας που περιλαμβάνει πληροφορίες έκδοσης. Καθώς και πληροφορίες έκδοσης στο χώρο ονομάτων σύμβασης υπηρεσίας επιτρέπει υπηρεσίες για την απομόνωση σημαντικών αλλαγών ορίζοντας μια νέα υπηρεσία σύμβαση με έναν νέο χώρο ονομάτων και την έκθεση σε ένα νέο τελικό σημείο. Με αυτόν τον τρόπο, οι υπολογιστές-πελάτες να συνεχίσετε να χρησιμοποιείτε το παλιό συμβόλαιο υπηρεσίας χωρίς να χρειάζεται να ενημερωθούν. Πληροφορίες έκδοσης μπορούν να αποτελούνται από μια ημερομηνία ή έναν αριθμό build. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υπηρεσία διαχείρισης εκδόσεων](http://go.microsoft.com/fwlink/?LinkID=180498). Για τους σκοπούς αυτής της εκμάθησης, το συνδυασμό ονομασίας του χώρου ονομάτων σύμβασης υπηρεσίας δεν περιέχει πληροφορίες έκδοσης.

1. Μέσα σε το `IEchoContract` περιβάλλοντος εργασίας, να δηλώσετε μια μέθοδο για τη λειτουργία μόνο το `IEchoContract` σύμβασης εκθέτει στο περιβάλλον εργασίας και να εφαρμόσετε το `OperationContractAttribute` χαρακτηριστικών τη μέθοδο που θέλετε για την έκθεση ως μέρος της δημόσιας υπηρεσίας Bus σύμβασης.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Αμέσως μετά το `IEchoContract` περιβάλλοντος εργασίας του ορισμού, να δηλώσετε ένα κανάλι που μεταβιβάζονται από και τα δύο `IEchoContract` και επίσης για το `IClientChannel` περιβάλλοντος εργασίας, όπως φαίνεται εδώ:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Ένα κανάλι είναι το αντικείμενο WCF μέσω του οποίου ο κεντρικός υπολογιστής και το πρόγραμμα-πελάτη μεταφέρουν πληροφορίες μεταξύ τους. Αργότερα, θα μπορείτε να συντάξετε κώδικα σε σχέση με το κανάλι για να εμφανίσετε πληροφορίες μεταξύ των δύο εφαρμογών.

1. Από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύσης** ή πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** για να επιβεβαιώσετε την ακρίβεια της εργασίας σας μέχρι στιγμής.

### <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας εμφανίζει ένα βασικό περιβάλλον εργασίας που ορίζει ένα συμβόλαιο Bus υπηρεσίας.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Τώρα που δημιουργείται το περιβάλλον εργασίας, μπορείτε να υλοποιήσετε το περιβάλλον εργασίας.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Υλοποίηση της σύμβασης WCF για να χρησιμοποιήσετε Bus υπηρεσίας

Δημιουργία μιας υπηρεσίας Bus μετάδοση απαιτεί να δημιουργήσετε πρώτα τη σύμβαση, η οποία έχει οριστεί χρησιμοποιώντας ένα περιβάλλον εργασίας. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία του περιβάλλοντος εργασίας, ανατρέξτε στο θέμα το προηγούμενο βήμα. Το επόμενο βήμα είναι να υλοποιήσετε το περιβάλλον εργασίας. Αυτό περιλαμβάνει τη δημιουργία μιας κλάσης με το όνομα `EchoService` που υλοποιεί ορίζεται από το χρήστη `IEchoContract` περιβάλλοντος εργασίας. Μετά την υλοποίηση το περιβάλλον εργασίας, μπορείτε, στη συνέχεια, να ρυθμίσετε το περιβάλλον εργασίας χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων App.config. Το αρχείο ρύθμισης παραμέτρων περιέχει τις απαραίτητες πληροφορίες για την εφαρμογή, όπως το όνομα της υπηρεσίας, το όνομα της σύμβασης, καθώς και τον τύπο του πρωτοκόλλου που χρησιμοποιείται για την επικοινωνία με υπηρεσία Bus. Ο κωδικός που χρησιμοποιείται για αυτές τις εργασίες παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία. Για μια πιο γενική συζήτηση σχετικά με τον τρόπο για την υλοποίηση της σύμβασης εξυπηρέτησης πελατών, ανατρέξτε στο θέμα [Εφαρμογή συμβόλαια](https://msdn.microsoft.com/library/ms733764.aspx) στην τεκμηρίωση WCF.

1. Δημιουργία μιας νέας κλάσης με το όνομα `EchoService` αμέσως μετά τον ορισμό των το `IEchoContract` περιβάλλοντος εργασίας. Το `EchoService` κλάση υλοποιεί το `IEchoContract` περιβάλλοντος εργασίας. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Παρόμοια με άλλες εφαρμογές του περιβάλλοντος εργασίας, μπορείτε να εφαρμόσετε τον ορισμό σε ένα άλλο αρχείο. Ωστόσο, για αυτό το πρόγραμμα εκμάθησης, η εφαρμογή βρίσκεται στο ίδιο αρχείο ως τον ορισμό του περιβάλλοντος εργασίας και το `Main` μέθοδο.

1. Το χαρακτηριστικό [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) για να εφαρμόσετε το `IEchoContract` περιβάλλοντος εργασίας. Το χαρακτηριστικό καθορίζει το όνομα της υπηρεσίας και χώρο ονομάτων. Μετά από αυτή την περίπτωση, η `EchoService` κλάσης εμφανίζεται ως εξής:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Υλοποίηση του `Echo` μέθοδο ορίζεται στο το `IEchoContract` περιβάλλοντος εργασίας του στο το `EchoService` τάξης. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία λύση** για να επιβεβαιώσετε την ακρίβεια της εργασίας σας.

### <a name="to-define-the-configuration-for-the-service-host"></a>Για να ορίσετε τη ρύθμιση παραμέτρων για τον κεντρικό υπολογιστή υπηρεσίας

1. Το αρχείο ρύθμισης παραμέτρων είναι παρόμοια με ένα αρχείο ρύθμισης παραμέτρων WCF. Περιλαμβάνει το όνομα της υπηρεσίας, τελικού σημείου (δηλαδή, τη θέση στην υπηρεσία Bus εκθέτει για προγράμματα-πελάτες και hosts για να επικοινωνούν μεταξύ τους) και τη σύνδεση (ο τύπος του πρωτοκόλλου που χρησιμοποιείται για την επικοινωνία). Η κύρια διαφορά είναι ότι αυτό το τελικό σημείο έχει ρυθμιστεί υπηρεσία αναφέρεται σε μια σύνδεση [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) , το οποίο δεν είναι μέρος του .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) είναι μία από τις συνδέσεις που ορίζονται από Bus υπηρεσίας.

1. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ στο αρχείο App.config για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio.

2. Στο το `<appSettings>` στοιχείο, αντικαταστήστε τα σύμβολα κράτησης θέσης με το όνομα του πεδίου υπηρεσίας ονομάτων και τις συσχετίσεις Ασφαλείας κλειδί που αντιγράψατε σε ένα προηγούμενο βήμα. 

1. Εντός του `<system.serviceModel>` ετικετών, προσθέστε μια `<services>` στοιχείο. Μπορείτε να ορίσετε πολλές εφαρμογές υπηρεσίας Bus σε ένα αρχείο ρύθμισης παραμέτρων μία. Ωστόσο, αυτό το πρόγραμμα εκμάθησης καθορίζει μόνο μία.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Μέσα σε το `<services>` στοιχείο, προσθέστε μια `<service>` στοιχείο για να καθορίσετε το όνομα της υπηρεσίας.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Εντός του `<service>` στοιχείο, ορίστε τη θέση της σύμβασης τελικού σημείου, καθώς και τον τύπο της σύνδεσης για το τελικό σημείο.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Το τελικό σημείο καθορίζει όπου θα είναι το πρόγραμμα-πελάτη για την εφαρμογή κεντρικού υπολογιστή. Αργότερα, το πρόγραμμα εκμάθησης χρησιμοποιεί αυτό το βήμα για να δημιουργήσετε ένα URI που εκθέτει πλήρως τον κεντρικό υπολογιστή μέσω Bus υπηρεσίας. Η σύνδεση δηλώνει ότι χρησιμοποιούμε TCP ως το πρωτόκολλο για την επικοινωνία με υπηρεσία Bus.

1. Από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύση** για να επιβεβαιώσετε την ακρίβεια της εργασίας σας.

### <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας εμφανίζει την εφαρμογή της σύμβασης υπηρεσίας.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Ο ακόλουθος κώδικας εμφανίζει τη βασική μορφή του αρχείου App.config που σχετίζεται με τον κεντρικό υπολογιστή υπηρεσίας.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Φιλοξενία και να εκτελέσετε μια βασική υπηρεσία Web για την καταχώρηση με Bus υπηρεσίας

Αυτό το βήμα περιγράφει τον τρόπο για να εκτελέσετε μια βασική υπηρεσία Bus υπηρεσίας.

### <a name="to-create-the-service-bus-credentials"></a>Για να δημιουργήσετε τα διαπιστευτήρια Bus υπηρεσίας

1. Στο `Main()`, να δημιουργήσετε δύο μεταβλητές στην οποία θέλετε να αποθηκεύσετε το χώρο ονομάτων και τις συσχετίσεις Ασφαλείας κλειδιού που διαβάζονται από το παράθυρο της κονσόλας.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Το κλειδί συσχετισμών Ασφαλείας θα χρησιμοποιηθεί αργότερα για πρόσβαση στο έργο σας Bus υπηρεσίας. Ο χώρος ονομάτων μεταβιβάζεται ως μια παράμετρος να `CreateServiceUri` για να δημιουργήσετε μια υπηρεσία URI.

4. Χρήση ενός αντικειμένου [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) , δηλώνουν ότι πρόκειται να χρησιμοποιήσετε έναν αριθμό-κλειδί συσχετισμών Ασφαλείας ως τον τύπο των διαπιστευτηρίων. Προσθέστε τον ακόλουθο κώδικα αμέσως μετά τον κωδικό που προσθέσατε στο τελευταίο βήμα.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Για να δημιουργήσετε μια βασική διεύθυνση για την υπηρεσία

1. Ακολουθεί τον κωδικό που προσθέσατε στο τελευταίο βήμα, δημιουργήστε μια `Uri` παρουσία για τη βασική διεύθυνση της υπηρεσίας. Αυτό URI Καθορίζει το συνδυασμό Bus υπηρεσία, το χώρο ονομάτων και τη διαδρομή του περιβάλλοντος εργασίας υπηρεσίας.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "μικρές" είναι συντομογραφία για το συνδυασμό Bus υπηρεσίας και υποδεικνύει ότι χρησιμοποιούμε TCP ως το πρωτόκολλο. Αυτό ήταν επίσης προηγουμένως που αναφέρονται στο αρχείο ρύθμισης παραμέτρων, όταν [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) έχει καθοριστεί ως η σύνδεση.
    
    Για αυτό το πρόγραμμα εκμάθησης, είναι το URI `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους του κεντρικού υπολογιστή υπηρεσίας

1. Ορίστε τη λειτουργία συνδεσιμότητας σε `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Στη λειτουργία συνδεσιμότητας περιγράφει το πρωτόκολλο που χρησιμοποιεί την υπηρεσία για την επικοινωνία με υπηρεσία Bus; HTTP ή TCP. Χρησιμοποιώντας την προεπιλεγμένη ρύθμιση `AutoDetect`, η υπηρεσία επιχειρεί να συνδεθεί με υπηρεσία Bus μέσω TCP εάν είναι διαθέσιμο και HTTP εάν TCP δεν είναι διαθέσιμη. Σημειώστε ότι αυτό διαφέρει από το πρωτόκολλο της υπηρεσίας καθορίζει για την επικοινωνία του προγράμματος-πελάτη. Πρωτόκολλο που καθορίζεται από τη σύνδεση που χρησιμοποιείται. Για παράδειγμα, μια υπηρεσία να χρησιμοποιήσετε τη σύνδεση [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) , η οποία καθορίζει ότι το τελικό σημείο (εκτίθεται στην υπηρεσία Bus) επικοινωνεί με προγράμματα-πελάτες μέσω HTTP. Ίδια υπηρεσία μπορούσατε να καθορίσετε **ConnectivityMode.AutoDetect** , έτσι ώστε η υπηρεσία επικοινωνεί με υπηρεσία Bus μέσω TCP.

1. Δημιουργήστε τον κεντρικό υπολογιστή υπηρεσίας, χρησιμοποιώντας το URI που δημιουργήσατε νωρίτερα σε αυτήν την ενότητα.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Ο κεντρικός υπολογιστής υπηρεσίας είναι το αντικείμενο WCF που εμφανίζει την υπηρεσία. Εδώ, περάσετε την τον τύπο της υπηρεσίας που θέλετε να δημιουργήσετε (μια `EchoService` τύπο), και επίσης στη διεύθυνση στην οποία θέλετε να εμφανίσετε την υπηρεσία.

1. Στο επάνω μέρος του αρχείου Program.cs, προσθέστε αναφορές σε [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) και [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Πίσω στο `Main()`, ρύθμιση παραμέτρων το τελικό σημείο για να ενεργοποιήσετε την πρόσβαση του κοινού.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Αυτό το βήμα πληροφορεί Bus υπηρεσίας που μπορείτε να βρείτε την εφαρμογή σας στο κοινό εξετάζοντας το υπηρεσίας ATOM Bus τροφοδοσίας για το έργο σας. Εάν ορίσετε **DiscoveryType** σε **ιδιωτικό**, ένα πρόγραμμα-πελάτη θα έχετε τη δυνατότητα να αποκτήσετε πρόσβαση στην υπηρεσία. Ωστόσο, η υπηρεσία δεν θα εμφανίζονται όταν κάνει αναζήτηση στο χώρο ονομάτων Bus υπηρεσίας. Αντί για αυτό, ο υπολογιστής-πελάτης θα πρέπει να γνωρίζετε εκ των προτέρων τη διαδρομή τελικού σημείου.

1. Ισχύουν τα διαπιστευτήρια υπηρεσίας για τα τελικά σημεία υπηρεσίας που ορίζονται στο αρχείο App.config:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Όπως αναφέρθηκε στο προηγούμενο βήμα, θα μπορούσε να έχουν δηλωθεί πολλές υπηρεσίες και τελικών σημείων στο αρχείο παραμέτρων. Εάν είχατε, αυτός ο κωδικός θα αλλάξουν το αρχείο ρύθμισης παραμέτρων και κάντε αναζήτηση για κάθε τελικό σημείο στο οποίο θα πρέπει να εφαρμόζεται τα διαπιστευτήριά σας. Ωστόσο, για αυτό το πρόγραμμα εκμάθησης, το αρχείο ρύθμισης παραμέτρων έχει μόνο μία τελικού σημείου.

### <a name="to-open-the-service-host"></a>Για να ανοίξετε τον κεντρικό υπολογιστή υπηρεσίας

1. Ανοίξτε την υπηρεσία.

    ```
    host.Open();
    ```

1. Ο χρήστης θα πρέπει να σας ενημερώσει ότι εκτελείται η υπηρεσία και εξηγούν τον τρόπο για να τερματίσετε την υπηρεσία.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Όταν ολοκληρώσετε, κλείστε τον κεντρικό υπολογιστή υπηρεσίας.

    ```
    host.Close();
    ```

1. Πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** για να δημιουργήσετε το έργο.

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα περιλαμβάνει την υπηρεσία σύμβασης και την εφαρμογή από τα προηγούμενα βήματα στην εκμάθηση και φιλοξενεί την υπηρεσία σε μια εφαρμογή κονσόλας. Η μεταγλώττιση τα εξής σε ένα εκτελέσιμο αρχείο με το όνομα EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Δημιουργήστε ένα πρόγραμμα-πελάτη WCF για το συμβόλαιο υπηρεσίας

Το επόμενο βήμα είναι να δημιουργήσετε μια βασική εφαρμογή προγράμματος-πελάτη Bus υπηρεσίας και να ορίσετε τη σύμβαση εξυπηρέτησης πελατών που θα εφαρμόσετε στα επόμενα βήματα. Σημείωση πολλά από τα παρακάτω βήματα μοιάζουν με τα βήματα που χρησιμοποιούνται για τη δημιουργία μιας υπηρεσίας: Ορίζει ένα συμβόλαιο, επεξεργασία App.config ένα αρχείο, χρησιμοποιώντας τα διαπιστευτήρια για σύνδεση σε υπηρεσία Bus, και ούτω καθεξής. Ο κωδικός που χρησιμοποιείται για αυτές τις εργασίες παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία.

1. Δημιουργία νέου έργου στην τρέχουσα λύση Visual Studio για το πρόγραμμα-πελάτη, κάνοντας τα εξής:
    1. Στην Εξερεύνηση λύσεων, στην ίδια λύση που περιέχει την υπηρεσία, κάντε δεξί κλικ στην τρέχουσα λύση (μην το έργο) και κάντε κλικ στην επιλογή **Προσθήκη**. Στη συνέχεια, κάντε κλικ στην επιλογή **Νέο έργο**.
    2. Στο παράθυρο διαλόγου **Προσθήκη νέου έργου** , κάντε κλικ στην επιλογή **Visual C#** (εάν **Visual C#** δεν εμφανίζεται, αναζητήστε στην περιοχή **Άλλες γλώσσες**), επιλέξτε το πρότυπο **Εφαρμογής κονσόλας** και ονομάστε το **EchoClient**.
    3. Κάντε κλικ στο **κουμπί OK**.
<br />

1. Στην Εξερεύνηση λύσεων, κάντε διπλό κλικ στο αρχείο Program.cs στο **EchoClient** έργου για να το ανοίξετε στο πρόγραμμα επεξεργασίας, εάν δεν είναι ήδη ανοιχτό.

1. Αλλάξτε το όνομα χώρου ονομάτων από το προεπιλεγμένο όνομα του `EchoClient` σε `Microsoft.ServiceBus.Samples`.

1. Εγκαταστήστε το [πακέτο NuGet Bus υπηρεσίας](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **EchoClient** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**. Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus`. Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης.

    ![][3]

1. Προσθήκη μιας `using` δήλωση για το χώρο ονομάτων [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) στο αρχείο Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Προσθέστε τον ορισμό σύμβασης υπηρεσίας για το χώρο ονομάτων, όπως φαίνεται στο παρακάτω παράδειγμα. Σημειώστε ότι ο ορισμός είναι ίδιος με τον ορισμό που χρησιμοποιούνται στο έργο **υπηρεσίας** . Θα πρέπει να προσθέσετε αυτόν τον κώδικα στο επάνω μέρος του `Microsoft.ServiceBus.Samples` χώρο ονομάτων.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** για να δημιουργήσετε το πρόγραμμα-πελάτη.

### <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας εμφανίζει την τρέχουσα κατάσταση του αρχείου Program.cs του EchoClient έργου.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Ρύθμιση παραμέτρων του προγράμματος-πελάτη WCF

Σε αυτό το βήμα, δημιουργείτε ένα αρχείο App.config για μια εφαρμογή προγράμματος-πελάτη βασικές που αποκτά πρόσβαση στην υπηρεσία που δημιουργήσατε προηγουμένως σε αυτό το πρόγραμμα εκμάθησης. Αυτό το αρχείο App.config Καθορίζει τη σύμβαση, σύνδεσης, και το όνομα του τελικού σημείου. Ο κωδικός που χρησιμοποιείται για αυτές τις εργασίες παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία.

1. Στην Εξερεύνηση λύσεων, μέσα στο έργο **EchoClient** , κάντε διπλό κλικ **App.config** για να ανοίξετε το αρχείο στο πρόγραμμα επεξεργασίας Visual Studio.

2. Στο το `<appSettings>` στοιχείο, αντικαταστήστε τα σύμβολα κράτησης θέσης με το όνομα του πεδίου υπηρεσίας ονομάτων και τις συσχετίσεις Ασφαλείας κλειδί που αντιγράψατε σε ένα προηγούμενο βήμα.

1. Μέσα στο στοιχείο system.serviceModel, προσθέστε μια `<client>` στοιχείο.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Αυτό το βήμα δηλώνει ότι ορίζετε μια εφαρμογή προγράμματος-πελάτη WCF στυλ.

1. Εντός του `client` στοιχείο, ορίστε το όνομα, σύμβασης και τύπο σύνδεσης για το τελικό σημείο.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Αυτό το βήμα Καθορίζει το όνομα του τελικού σημείου, το συμβόλαιο που ορίζονται από την υπηρεσία και το γεγονός ότι η εφαρμογή προγράμματος-πελάτη χρησιμοποιεί TCP για την επικοινωνία με υπηρεσία Bus. Για να συνδέσετε αυτήν τη ρύθμιση παραμέτρων τελικού σημείου με την υπηρεσία URI, χρησιμοποιείται το όνομα τελικού σημείου στο επόμενο βήμα.

1. Κάντε κλικ στην επιλογή **αρχείο**, στη συνέχεια, **Αποθήκευση όλων**.

## <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας εμφανίζει το αρχείο App.config για το πρόγραμμα-πελάτη Αντήχηση.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Υλοποίηση το πρόγραμμα-πελάτη WCF για να καλέσετε Bus υπηρεσίας

Σε αυτό το βήμα, μπορείτε να εφαρμόσετε μια εφαρμογή προγράμματος-πελάτη βασικές που αποκτά πρόσβαση στην υπηρεσία που δημιουργήσατε προηγουμένως σε αυτό το πρόγραμμα εκμάθησης. Παρόμοια με την υπηρεσία, ο υπολογιστής-πελάτης εκτελεί πολλές από τις ίδιες λειτουργίες για να αποκτήσετε πρόσβαση Bus υπηρεσίας:

1. Ορίζει τη λειτουργία σύνδεσης.
1. Δημιουργεί το URI που εντοπίζει της υπηρεσίας κεντρικού υπολογιστή.
1. Καθορίζει τα διαπιστευτήρια ασφαλείας.
1. Ισχύει τα διαπιστευτήρια για τη σύνδεση.
1. Ανοίγει η σύνδεση.
1. Εκτελεί τη συγκεκριμένη εφαρμογή εργασίες.
1. Κλείσιμο της σύνδεσης.

Ωστόσο, μία από τις κύριες διαφορές είναι ότι η εφαρμογή προγράμματος-πελάτη χρησιμοποιεί ένα κανάλι για σύνδεση σε υπηρεσία Bus, ότι η υπηρεσία χρησιμοποιεί μια κλήση σε **ServiceHost**. Ο κωδικός που χρησιμοποιείται για αυτές τις εργασίες παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία.

### <a name="to-implement-a-client-application"></a>Για να υλοποιήσετε μια εφαρμογή προγράμματος-πελάτη

1. Ορίστε τη λειτουργία σύνδεσης για **τον αυτόματο εντοπισμό**. Προσθέστε τον ακόλουθο κώδικα στο εσωτερικό του `Main()` από την εφαρμογή **EchoClient** .

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Ορισμός μεταβλητών για τη διατήρηση τις τιμές για το χώρο ονομάτων υπηρεσίας, και το κλειδί συσχετισμών Ασφαλείας που διαβάζονται από την κονσόλα.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Δημιουργήστε το URI που καθορίζει τη θέση του κεντρικού υπολογιστή στο έργο σας Bus υπηρεσίας.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Δημιουργήστε το αντικείμενο διαπιστευτηρίων για το τελικό σημείο χώρος ονομάτων υπηρεσίας.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Δημιουργία της προέλευσης καναλιού που φορτώνει τις ρυθμίσεις παραμέτρων που περιγράφονται στο αρχείο App.config.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Μιας προέλευσης καναλιών είναι ένα αντικείμενο WCF που δημιουργεί ένα κανάλι μέσω του οποίου επικοινωνία τις εφαρμογές υπηρεσιών και προγράμματος-πελάτη.

1. Εφαρμόστε τα διαπιστευτήρια Bus υπηρεσίας.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Δημιουργήστε και ανοίξτε το κανάλι για την υπηρεσία.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Συντάξτε το βασικό περιβάλλον χρήστη και τη λειτουργικότητα για την αντήχηση.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Σημειώστε ότι ο κώδικας χρησιμοποιεί την παρουσία του αντικειμένου καναλιού με ένα διακομιστή μεσολάβησης για την υπηρεσία.

1. Κλείστε το κανάλι και κλείστε το factory.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Για να εκτελέσετε τις εφαρμογές

1. Πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** για να δημιουργήσετε τη λύση. Αυτό δημιουργεί το έργο προγράμματος-πελάτη και του έργου υπηρεσίας που δημιουργήσατε στα προηγούμενα βήματα.

2. Πριν να εκτελέσετε την εφαρμογή-πελάτη, βεβαιωθείτε ότι εκτελείται η εφαρμογή υπηρεσίας. Στην Εξερεύνηση λύσεων στο Visual Studio, κάντε δεξί κλικ της λύσης **EchoService** και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**.

3. Στο παράθυρο διαλόγου Ιδιότητες λύσης, κάντε κλικ στην επιλογή **Project εκκίνησης**και, στη συνέχεια, κάντε κλικ στο κουμπί **πολλά έργα εκκίνησης** . Βεβαιωθείτε ότι **EchoService** εμφανίζεται πρώτη στη λίστα. 

4. Ορίστε το πλαίσιο **ενέργειας** για το **EchoService** και το **EchoClient** έργων για να **ξεκινήσετε**.

    ![][5]

5. Κάντε κλικ στην επιλογή **εξαρτήσεις μεταξύ έργων**. Στο πλαίσιο **έργα** , επιλέξτε **EchoClient**. Στο πλαίσιο **ανάλογα με** , βεβαιωθείτε ότι είναι επιλεγμένο το στοιχείο **EchoService** .

    ![][6]

6. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου **Ιδιότητες** .

7. Πατήστε το πλήκτρο **F5** για να εκτελέσετε και τα δύο έργα.

8. Και τα δύο παράθυρα κονσόλας ανοίξτε και ζητά το όνομα χώρου ονομάτων. Η υπηρεσία πρέπει να εκτελέσετε πρώτο, επομένως στο παράθυρο της κονσόλας **EchoService** , πληκτρολογήστε το χώρο ονομάτων και, στη συνέχεια, πατήστε το πλήκτρο **Enter**.

9. Στη συνέχεια, θα σας ζητηθεί για τον αριθμό-κλειδί συσχετισμών Ασφαλείας. Εισαγάγετε τον αριθμό-κλειδί συσχετισμών Ασφαλείας και πατήστε το πλήκτρο ENTER.

    Ακολουθεί παράδειγμα έξοδο από το παράθυρο της κονσόλας. Σημειώστε ότι οι τιμές που παρέχονται για παράδειγμα ακολουθούν μόνο.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Η εφαρμογή υπηρεσίας εκτυπώνει στο παράθυρο κονσόλας τη διεύθυνση στην οποία ακρόαση, όπως φαίνεται στο παρακάτω παράδειγμα.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Στο παράθυρο της κονσόλας **EchoClient** , εισαγάγετε τις ίδιες πληροφορίες που εισαγάγατε προηγουμένως για την εφαρμογή υπηρεσίας. Ακολουθήστε τα προηγούμενα βήματα για να εισαγάγετε το ίδιο χώρο ονομάτων υπηρεσίας και τις τιμές κλειδιού συσχετισμών Ασφαλείας για την εφαρμογή-πελάτη.

11. Μετά την εισαγωγή αυτές τις τιμές, ο υπολογιστής-πελάτης ανοίγει ένα κανάλι για την υπηρεσία και σας ζητά να εισαγάγετε κάποιο κείμενο, όπως φαίνεται στο παρακάτω παράδειγμα εξόδου κονσόλας.

    `Enter text to echo (or [Enter] to exit):` 

    Πληκτρολογήστε κάποιο κείμενο για την αποστολή στην εφαρμογή υπηρεσίας και πατήστε το πλήκτρο Enter. Αυτό το κείμενο αποστέλλεται στην υπηρεσία μέσω στη λειτουργία της υπηρεσίας αντήχηση και εμφανίζεται στο παράθυρο της κονσόλας υπηρεσία όπως το ακόλουθο παράδειγμα αποτέλεσμα.

    `Echoing: My sample text`

    Η εφαρμογή υπολογιστή-πελάτη λαμβάνει την τιμή επιστροφής από το `Echo` πράξη, η οποία είναι το αρχικό κείμενο και εκτυπώνει το παράθυρο κονσόλας. Ακολουθεί παράδειγμα έξοδο από το παράθυρο κονσόλας προγράμματος-πελάτη.

    `Server echoed: My sample text`

12. Μπορείτε να συνεχίσετε την αποστολή μηνυμάτων κειμένου από το πρόγραμμα-πελάτη στην υπηρεσία με αυτόν τον τρόπο. Όταν τελειώσετε, πατήστε το πλήκτρο Enter στο windows κονσόλας προγράμματος-πελάτη και της υπηρεσίας για να τερματίσετε δύο εφαρμογές.

## <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη, πώς μπορείτε να καλέσετε τις λειτουργίες της υπηρεσίας και πώς μπορείτε να κλείσετε το πρόγραμμα-πελάτη μετά την ολοκλήρωση της λειτουργίας κλήσης.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Επόμενα βήματα

Αυτό το πρόγραμμα εκμάθησης εμφάνιζε πώς να δημιουργείτε ένα πρόγραμμα-πελάτη Bus υπηρεσίας εφαρμογών και υπηρεσιών χρησιμοποιώντας τις δυνατότητες "μετάδοση" Bus υπηρεσίας. Για ένα πρόγραμμα εκμάθησης παρόμοια που χρησιμοποιεί Bus υπηρεσία [ανταλλαγής μηνυμάτων](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging), ανατρέξτε στο θέμα της [Υπηρεσίας Bus όπου μεσολαβούν μηνυμάτων .NET πρόγραμμα εκμάθησης](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Για να μάθετε περισσότερα σχετικά με την υπηρεσία Bus, ανατρέξτε στα ακόλουθα θέματα.

- [Επισκόπηση μηνυμάτων Bus υπηρεσίας](../service-bus-messaging/service-bus-messaging-overview.md)
- [Υπηρεσία Bus τα θεμελιώδη στοιχεία](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Αρχιτεκτονική Bus υπηρεσίας](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
