<properties
    pageTitle="Υπηρεσία Bus ΥΠΌΛΟΙΠΑ προγραμμάτων εκμάθησης χρησιμοποιώντας αναμετάδοση μηνυμάτων | Microsoft Azure"
    description="Δημιουργήστε μια απλή εφαρμογή κεντρικού υπολογιστή αναμετάδοσης Bus υπηρεσίας που εκθέτει ένα περιβάλλον που βασίζεται στο ΥΠΌΛΟΙΠΟ."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Πρόγραμμα εκμάθησης ΥΠΌΛΟΙΠΑ μεταγωγής Bus υπηρεσίας

Αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να δημιουργήσετε μια απλή εφαρμογή κεντρικού υπολογιστή Bus υπηρεσίας που εκθέτει ένα περιβάλλον που βασίζεται στο ΥΠΌΛΟΙΠΟ. ΥΠΌΛΟΙΠΟ ενεργοποιεί ένα πρόγραμμα-πελάτη web, όπως ένα πρόγραμμα περιήγησης web, για να αποκτήσετε πρόσβαση τα API Bus υπηρεσίας μέσω HTTP αιτήσεις.

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί τα ΥΠΌΛΟΙΠΑ Windows Communication Foundation (WCF) μοντέλο προγραμματισμού για να δημιουργήσετε μια υπηρεσία ΥΠΌΛΟΙΠΟ στην υπηρεσία Bus. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Μοντέλο προγραμματισμού ΥΠΌΛΟΙΠΑ WCF](https://msdn.microsoft.com/library/bb412169.aspx) και [Σχεδίαση και υλοποίηση υπηρεσίες](https://msdn.microsoft.com/library/ms729746.aspx) στην τεκμηρίωση WCF.

## <a name="step-1-create-a-service-namespace"></a>Βήμα 1: Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας

Το πρώτο βήμα είναι να δημιουργήσετε ένα χώρο ονομάτων και για να αποκτήσετε έναν αριθμό-κλειδί θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας). Ένας χώρος ονομάτων παρέχει όριο μιας εφαρμογής για κάθε εφαρμογή εκτίθεται μέσω Bus υπηρεσίας. Έναν αριθμό-κλειδί συσχετισμών Ασφαλείας δημιουργείται αυτόματα από το σύστημα όταν δημιουργείται ένα χώρο ονομάτων υπηρεσίας. Ο συνδυασμός χώρος ονομάτων υπηρεσίας και κλειδί συσχετισμών Ασφαλείας παρέχει τα διαπιστευτήρια για Bus Service για τον έλεγχο ταυτότητας πρόσβαση σε μια εφαρμογή του.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Βήμα 2: Ορίστε ένα συμβόλαιο υπηρεσία WCF που βασίζεται στο ΥΠΌΛΟΙΠΟ για χρήση με Bus υπηρεσίας

Ως με άλλες υπηρεσίες Bus υπηρεσίας, όταν δημιουργείτε μια υπηρεσία ΥΠΌΛΟΙΠΑ στυλ, πρέπει να ορίσετε τη σύμβαση. Η σύμβαση καθορίζει ποιες λειτουργίες υποστηρίζει τον κεντρικό υπολογιστή. Μια λειτουργία της υπηρεσίας μπορεί να θεωρηθεί ως μια μέθοδο υπηρεσίας web. Συμβάσεις δημιουργούνται καθορίζοντας ένα περιβάλλον εργασίας C++, C# ή Visual Basic. Κάθε μέθοδο στο περιβάλλον εργασίας αντιστοιχεί σε μια συγκεκριμένη λειτουργία τεχνικής υποστήριξης. Το χαρακτηριστικό [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) πρέπει να εφαρμοστεί σε κάθε περιβάλλον εργασίας και το χαρακτηριστικό [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) θα πρέπει να εφαρμοστεί σε κάθε εργασία. Εάν μια μέθοδο σε ένα περιβάλλον εργασίας που έχει το [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) δεν έχει το [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), δεν εκτίθεται αυτή τη μέθοδο. Εμφανίζεται ο κωδικός που χρησιμοποιείται για αυτές τις εργασίες στο παράδειγμα ακολουθώντας τη διαδικασία.

Η κύρια διαφορά μεταξύ συμβόλαιο βασικές Bus υπηρεσίας και ένα συμβόλαιο ΥΠΌΛΟΙΠΑ στυλ είναι η προσθήκη μιας ιδιότητας για να το [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Αυτή η ιδιότητα σάς επιτρέπει να αντιστοιχίσετε μια μέθοδο στο περιβάλλον σας σε μια μέθοδο στην άλλη πλευρά του περιβάλλοντος εργασίας. Σε αυτήν την περίπτωση, θα χρησιμοποιήσουμε [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) να συνδέσετε μια μέθοδο HTTP GET. Αυτό σας επιτρέπει Bus υπηρεσίας για να ανακτήσετε με ακρίβεια και να ερμηνεύσετε εντολές που αποστέλλονται στο περιβάλλον εργασίας του.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Για να δημιουργήσετε ένα συμβόλαιο υπηρεσίας Bus με ένα περιβάλλον εργασίας

1. Ανοίξτε το Visual Studio ως διαχειριστής: κάντε δεξί κλικ στο πρόγραμμα από το μενού **Έναρξη** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση ως διαχειριστής**.

2. Δημιουργήστε ένα νέο έργο εφαρμογής κονσόλας. Κάντε κλικ στο μενού **αρχείο** και επιλέξτε **Δημιουργία**και κατόπιν επιλέξτε το **στοιχείο έργο**. Στο παράθυρο διαλόγου **Νέο έργο** , κάντε κλικ στην επιλογή **Visual C#**, επιλέξτε το πρότυπο **Εφαρμογής κονσόλας** και ονομάστε το **ImageListener**. Χρησιμοποιήστε την προεπιλεγμένη **θέση**. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

3. Για ένα έργο C#, Visual Studio δημιουργεί μια `Program.cs` αρχείου. Αυτή η κλάση περιέχει ένα κενό `Main()` μέθοδο, που απαιτείται για ένα έργο εφαρμογής κονσόλας για να δημιουργήσετε σωστά.

4. Προσθέστε αναφορές σε υπηρεσία Bus και **System.ServiceModel.dll** στο έργο κατά την εγκατάσταση του πακέτου NuGet Bus υπηρεσίας. Αυτό το πακέτο προσθέτει αυτόματα αναφορές για τις βιβλιοθήκες Bus υπηρεσίας, καθώς και το WCF **System.ServiceModel**. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ImageListener** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**. Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus`. Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης.

5. Πρέπει να προσθέσετε ρητά μια αναφορά σε **System.ServiceModel.Web.dll** στο έργο:

    μια. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο φάκελο **αναφορές** κάτω από το φάκελο του έργου και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη αναφοράς**.

    β. Στο παράθυρο διαλόγου **Προσθήκη αναφοράς** , κάντε κλικ στην καρτέλα **πλαίσιο** στην αριστερή πλευρά και στο πλαίσιο **αναζήτησης** , πληκτρολογήστε **System.ServiceModel.Web**. Επιλέξτε το πλαίσιο ελέγχου **System.ServiceModel.Web** και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

6. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) είναι το πεδίο ονόματος που επιτρέπει την πρόσβαση μέσω προγραμματισμού σε βασικές δυνατότητες του WCF. Υπηρεσία Bus χρησιμοποιεί πολλά από τα αντικείμενα και χαρακτηριστικά του WCF για να ορίσετε συμβόλαια. Θα χρησιμοποιήσετε αυτόν το χώρο ονομάτων στις περισσότερες από τις εφαρμογές σας Bus υπηρεσίας αναμετάδοσης. Ομοίως, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) βοηθά στον ορισμό του καναλιού, το οποίο είναι το αντικείμενο μέσω του οποίου επικοινωνία με υπηρεσία Bus και το πρόγραμμα περιήγησης web του προγράμματος-πελάτη. Τέλος, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) περιέχει τους τύπους που σας επιτρέπουν να δημιουργήσετε εφαρμογές που βασίζονται στο web.

7. Μετονομάστε το `ImageListener` χώρος ονομάτων για να **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Απευθείας αφού ορίσετε άγκιστρο ανοίγματος της δήλωσης χώρο ονομάτων, ένα νέο περιβάλλον εργασίας με το όνομα **IImageContract** και να εφαρμόσετε το χαρακτηριστικό **ServiceContractAttribute** το περιβάλλον εργασίας με μια τιμή του `http://samples.microsoft.com/ServiceModel/Relay/`. Η τιμή του χώρου ονομάτων διαφέρει από το χώρο ονομάτων που χρησιμοποιείτε σε όλο το εύρος του κώδικα. Η τιμή του χώρου ονομάτων χρησιμοποιείται ως ένα μοναδικό αναγνωριστικό για αυτήν τη σύμβαση, και θα πρέπει να έχετε πληροφορίες έκδοσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υπηρεσία διαχείρισης εκδόσεων](http://go.microsoft.com/fwlink/?LinkID=180498). Καθορίζει το χώρο ονομάτων ρητά αποτρέπει την προεπιλεγμένη τιμή χώρος ονομάτων προστίθενται στο όνομα της σύμβασης.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Μέσα σε το `IImageContract` περιβάλλοντος εργασίας, να δηλώσετε μια μέθοδο για τη λειτουργία μόνο το `IImageContract` σύμβασης εκθέτει στο περιβάλλον εργασίας και να εφαρμόσετε το `OperationContractAttribute` χαρακτηριστικών τη μέθοδο που θέλετε για την έκθεση ως μέρος της δημόσιας υπηρεσίας Bus σύμβασης.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Στο χαρακτηριστικό **OperationContract** , προσθέστε την τιμή **WebGet** .

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Αυτή η ενέργεια επιτρέπει την υπηρεσία Bus να δρομολόγηση HTTP GET αιτήσεις `GetImage`, και για να μεταφράσετε τις τιμές επιστροφής της `GetImage` σε μια απάντηση HTTP GETRESPONSE. Παρακάτω σε το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε ένα πρόγραμμα περιήγησης web για πρόσβαση σε αυτήν τη μέθοδο και για να εμφανίσετε την εικόνα στο πρόγραμμα περιήγησης.

11. Αμέσως μετά το `IImageContract` ορισμού, δηλώσετε ένα κανάλι που μεταβιβάζονται από και τα δύο το `IImageContract` και `IClientChannel` διασυνδέσεων.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Ένα κανάλι είναι το αντικείμενο WCF μέσω του οποίου την υπηρεσία και το πρόγραμμα-πελάτη μεταφέρουν πληροφορίες μεταξύ τους. Αργότερα, θα μπορείτε να δημιουργήσετε το κανάλι στην εφαρμογή κεντρικού υπολογιστή. Υπηρεσία Bus χρησιμοποιεί, στη συνέχεια, αυτό το κανάλι για να μεταβιβάζουν τις αιτήσεις HTTP GET από το πρόγραμμα περιήγησης για την υλοποίηση της **GetImage** . Υπηρεσία Bus χρησιμοποιεί επίσης το κανάλι για να τραβήξετε την τιμή επιστροφής **GetImage** και αυτό μεταφράζεται σε μια GETRESPONSE HTTP για το πρόγραμμα περιήγησης του προγράμματος-πελάτη.

12. Από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύση** για να επιβεβαιώσετε την ακρίβεια της εργασίας σας μέχρι στιγμής.

### <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας εμφανίζει ένα βασικό περιβάλλον εργασίας που ορίζει ένα συμβόλαιο Bus υπηρεσίας.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Βήμα 3: Υλοποίηση της σύμβασης βασίζεται στο ΥΠΌΛΟΙΠΟ WCF εξυπηρέτησης πελατών για να χρησιμοποιήσετε Bus υπηρεσίας

Η δημιουργία μιας υπηρεσίας Bus υπηρεσίας ΥΠΌΛΟΙΠΑ στυλ απαιτεί να δημιουργήσετε πρώτα τη σύμβαση, η οποία έχει οριστεί χρησιμοποιώντας ένα περιβάλλον εργασίας. Το επόμενο βήμα είναι να υλοποιήσετε το περιβάλλον εργασίας. Αυτό περιλαμβάνει τη δημιουργία μιας κλάσης με το όνομα **ImageService** που υλοποιεί το περιβάλλον εργασίας χρήστη **IImageContract** . Μετά την υλοποίηση της σύμβασης, στη συνέχεια, να ρυθμίσετε το περιβάλλον εργασίας χρησιμοποιώντας ένα αρχείο App.config. Το αρχείο ρύθμισης παραμέτρων περιέχει τις απαραίτητες πληροφορίες για την εφαρμογή, όπως το όνομα της υπηρεσίας, το όνομα της σύμβασης, καθώς και τον τύπο του πρωτοκόλλου που χρησιμοποιείται για την επικοινωνία με υπηρεσία Bus. Ο κωδικός που χρησιμοποιείται για αυτές τις εργασίες παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία.

Όπως συμβαίνει με τα προηγούμενα βήματα, υπάρχει πολύ μικρή διαφορά μεταξύ της εφαρμογής ένα συμβόλαιο ΥΠΌΛΟΙΠΑ στυλ και μια βασική σύμβαση Bus υπηρεσίας.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Για να υλοποιήσετε ένα συμβόλαιο Bus υπηρεσίας ΥΠΌΛΟΙΠΑ στυλ

1. Δημιουργήστε μια νέα κλάση με το όνομα **ImageService** αμέσως μετά τον ορισμό του περιβάλλοντος εργασίας **IImageContract** . Η κλάση **ImageService** υλοποιεί το περιβάλλον εργασίας **IImageContract** .

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Παρόμοια με άλλες εφαρμογές του περιβάλλοντος εργασίας, μπορείτε να εφαρμόσετε τον ορισμό σε ένα άλλο αρχείο. Ωστόσο, για αυτό το πρόγραμμα εκμάθησης, εμφανίζεται η εφαρμογή στο ίδιο αρχείο ως τον ορισμό του περιβάλλοντος εργασίας και `Main()` μέθοδο.

2. Εφαρμόστε το χαρακτηριστικό [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) την κλάση **IImageService** για να υποδείξει ότι η κλάση είναι μια εφαρμογή της σύμβασης WCF.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Όπως προαναφέρθηκε, αυτός ο χώρος ονομάτων δεν είναι ένα παραδοσιακό χώρο ονομάτων. Αντί για αυτό, είναι τμήμα της αρχιτεκτονικής WCF που προσδιορίζει το συμβόλαιο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ονόματα σύμβασης δεδομένων](https://msdn.microsoft.com/library/ms731045.aspx) στην τεκμηρίωση WCF.

3. Προσθήκη μιας εικόνας .jpg στο έργο σας.  

    Αυτή είναι μια εικόνα που εμφανίζει την υπηρεσία στο πρόγραμμα περιήγησης λήψη. Κάντε δεξιό κλικ στο έργο σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**. Στη συνέχεια, κάντε κλικ στην επιλογή **Υπάρχον στοιχείο**. Χρησιμοποιήστε το παράθυρο διαλόγου **Προσθήκη υπάρχοντος στοιχείου** για να μεταβείτε σε μια κατάλληλη .jpg, και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

    Κατά την προσθήκη του αρχείου, βεβαιωθείτε ότι **Όλα τα αρχεία που** είναι επιλεγμένο στην αναπτυσσόμενη λίστα δίπλα στο το **όνομα αρχείου:** πεδίο. Τα υπόλοιπα αυτού του προγράμματος εκμάθησης προϋποθέτει ότι το όνομα της εικόνας είναι "του Image.jpg:". Εάν έχετε ένα διαφορετικό αρχείο, θα πρέπει να μετονομάσετε την εικόνα ή να αλλάξετε τον κωδικό για την αποζημίωση.

4. Για να βεβαιωθείτε ότι εκτελείται η υπηρεσία να βρείτε το αρχείο εικόνας, στην **Εξερεύνηση λύσεων** κάντε δεξιό κλικ στο αρχείο εικόνας και κατόπιν κάντε κλικ στην επιλογή **Ιδιότητες**. Στο παράθυρο **ιδιοτήτων** , ορίστε **Αντιγραφή κατάλογο εξόδου** αντίγραφο **Εάν νεότερη έκδοση**.

5. Προσθήκη αναφοράς σε συγκρότησης **System.Drawing.dll** στο έργο και προσθέστε επίσης τα ακόλουθα που σχετίζονται `using` δηλώσεις.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Στην τάξη **ImageService** , προσθέστε το παρακάτω κατασκευή που φορτώνει το bitmap και προετοιμάζει για να το στείλετε στο πρόγραμμα περιήγησης του προγράμματος-πελάτη.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Απευθείας μετά το προηγούμενο κώδικα, προσθέστε την ακόλουθη μέθοδο **GetImage** στην τάξη **ImageService** για να λάβετε ένα μήνυμα HTTP που περιέχει την εικόνα.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Αυτή η υλοποίηση χρησιμοποιεί **MemoryStream δεν** για να ανακτήσετε την εικόνα και προετοιμασία του για ροή στο πρόγραμμα περιήγησης. Το ξεκινά τη θέση ροή από το μηδέν, δηλώνει τη ροή περιεχομένου ως jpeg και ροές τις πληροφορίες.

8. Από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύσης**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Για να ορίσετε τη ρύθμιση παραμέτρων για την εκτέλεση της υπηρεσίας web στην υπηρεσία Bus

1. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio **App.config** .

    Το αρχείο **App.config** μοιάζει με ένα αρχείο ρύθμισης παραμέτρων WCF και περιλαμβάνει το όνομα της υπηρεσίας, τελικού σημείου (δηλαδή, τη θέση στην υπηρεσία Bus εκθέτει για προγράμματα-πελάτες και hosts για να επικοινωνούν μεταξύ τους) και σύνδεση (ο τύπος του πρωτοκόλλου που χρησιμοποιείται για την επικοινωνία). Η κύρια διαφορά εδώ είναι ότι το τελικό σημείο έχει ρυθμιστεί υπηρεσία αναφέρεται σε μια σύνδεση [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) , το οποίο δεν είναι μέρος του .NET Framework.

2. Το `<system.serviceModel>` στοιχείο XML είναι ένα στοιχείο WCF που ορίζει μία ή περισσότερες υπηρεσίες. Εδώ, χρησιμοποιείται για να καθορίσετε το όνομα της υπηρεσίας και τελικού σημείου. Στο κάτω μέρος του `<system.serviceModel>` στοιχείο (αλλά εξακολουθείτε να μέσα `<system.serviceModel>`), προσθέστε μια `<bindings>` στοιχείο που περιλαμβάνει το παρακάτω περιεχόμενο. Αυτό ορίζει τις συνδέσεις που χρησιμοποιούνται στην εφαρμογή. Μπορείτε να ορίσετε πολλές συνδέσεις, αλλά αυτό το πρόγραμμα εκμάθησης ορίζετε μόνο μία.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Αυτό το βήμα ορίζει μια σύνδεση υπηρεσίας Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) με **relayClientAuthenticationType** **καμία**. Αυτή η ρύθμιση υποδεικνύει ότι ένα τελικό σημείο χρησιμοποιώντας αυτήν τη σύνδεση δεν απαιτεί διαπιστευτήρια ενός προγράμματος-πελάτη.

3. Μετά την `<bindings>` στοιχείο, προσθέστε μια `<services>` στοιχείο. Παρόμοια με τις συνδέσεις, μπορείτε να ορίσετε πολλές υπηρεσίες σε ένα αρχείο ρύθμισης παραμέτρων μία. Ωστόσο, για αυτό το πρόγραμμα εκμάθησης, μπορείτε να ορίσετε μόνο μία.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Αυτό το βήμα ρυθμίζει τις παραμέτρους μιας υπηρεσίας που χρησιμοποιεί την προεπιλεγμένη παλιότερα καθορισμένη **webHttpRelayBinding**. Επίσης, χρησιμοποιεί την προεπιλεγμένη **sbTokenProvider**, που καθορίζεται στο επόμενο βήμα.

4. Μετά την `<services>` στοιχείο, δημιουργήστε μια `<behaviors>` στοιχείο με το παρακάτω περιεχόμενο, αντικαθιστώντας "SAS_KEY" με τον αριθμό-κλειδί *Θέσει σε κοινή χρήση υπογραφής Access* (συσχετισμών Ασφαλείας) που ήταν προηγουμένως που λαμβάνονται από την [πύλη του Azure][].

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Βρίσκεται ακόμη στο App.config, στο το `<appSettings>` στοιχείο, αντικατάσταση τιμή συμβολοσειράς ολόκληρη τη σύνδεση με τη συμβολοσειρά σύνδεσης που έχετε λάβει προηγουμένως από την πύλη. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύση** για τη δημιουργία της λύσης ολόκληρο.

### <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας εμφανίζει την εφαρμογή σύμβασης και της υπηρεσίας για μια υπηρεσία που βασίζεται στο ΥΠΌΛΟΙΠΟ που εκτελείται σε υπηρεσία Bus χρησιμοποιώντας τη σύνδεση **WebHttpRelayBinding** .

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Το παρακάτω παράδειγμα εμφανίζει το αρχείο App.config που σχετίζεται με την υπηρεσία.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Βήμα 4: Φιλοξενεί την υπηρεσία WCF που βασίζεται στο ΥΠΌΛΟΙΠΟ για να χρησιμοποιήσετε Bus υπηρεσίας

Αυτό το βήμα περιγράφει τον τρόπο για να εκτελέσετε μια υπηρεσία web, χρησιμοποιώντας μια εφαρμογή κονσόλας στην υπηρεσία Bus. Μια ολοκληρωμένη λίστα με τον κώδικα που έχει συνταχθεί σε αυτό το βήμα παρέχονται στο παράδειγμα ακολουθώντας τη διαδικασία.

### <a name="to-create-a-base-address-for-the-service"></a>Για να δημιουργήσετε μια βασική διεύθυνση για την υπηρεσία

1. Στο το `Main()` λειτουργία δήλωσης, δημιουργήστε μια μεταβλητή για να αποθηκεύσετε το χώρο ονομάτων του έργου σας Bus υπηρεσίας. Βεβαιωθείτε ότι για να αντικαταστήσετε `yourNamespace` με το όνομα του πεδίου υπηρεσίας ονομάτων που δημιουργήσατε προηγουμένως.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Υπηρεσία Bus χρησιμοποιεί το όνομα του πεδίου ονομάτων για να δημιουργήσετε μια μοναδική διεύθυνση URI.

2. Δημιουργία μιας `Uri` παρουσία για τη βασική διεύθυνση της υπηρεσίας που βασίζεται σε χώρο ονομάτων.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους του κεντρικού υπολογιστή υπηρεσίας web

- Δημιουργήστε τον κεντρικό υπολογιστή υπηρεσίας web, χρησιμοποιώντας τη διεύθυνση URI που δημιουργήσατε νωρίτερα σε αυτήν την ενότητα.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Ο κεντρικός υπολογιστής υπηρεσίας είναι το αντικείμενο WCF που εμφανίζει την εφαρμογή κεντρικού υπολογιστή. Αυτό το παράδειγμα μεταβιβάζει τον τύπο του κεντρικού υπολογιστή που θέλετε να δημιουργήσετε (μια **ImageService**), καθώς και τη διεύθυνση στην οποία θέλετε να εκθέσετε στην εφαρμογή κεντρικού υπολογιστή.

### <a name="to-run-the-web-service-host"></a>Για να εκτελέσετε τον κεντρικό υπολογιστή υπηρεσίας web

1. Ανοίξτε την υπηρεσία.

    ```
    host.Open();
    ```
    Τώρα εκτελείται η υπηρεσία.

2. Εμφανίζει ένα μήνυμα που δηλώνει ότι εκτελείται η υπηρεσία και πώς μπορείτε να διακόψετε την υπηρεσία.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Όταν ολοκληρώσετε, κλείστε τον κεντρικό υπολογιστή υπηρεσίας.

    ```
    host.Close();
    ```

## <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα περιλαμβάνει την υπηρεσία σύμβασης και την εφαρμογή από τα προηγούμενα βήματα στην εκμάθηση και φιλοξενεί την υπηρεσία σε μια εφαρμογή κονσόλας. Μεταγλώττιση τον ακόλουθο κώδικα σε ένα εκτελέσιμο αρχείο με το όνομα ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Μεταγλώττιση του κώδικα

Μετά τη δημιουργία της λύσης, κάντε τα εξής για να εκτελέσετε την εφαρμογή:

1. Πατήστε το πλήκτρο **F5**ή μεταβείτε στη θέση εκτελέσιμο αρχείο (ImageListener\bin\Debug\ImageListener.exe), για να εκτελέσετε την υπηρεσία. Διατήρηση της εφαρμογής που εκτελείται, καθώς αυτό είναι απαραίτητη για να εκτελέσετε το επόμενο βήμα.

2. Αντιγράψτε και επικολλήστε τη διεύθυνση από τη γραμμή εντολών σε ένα πρόγραμμα περιήγησης για να δείτε την εικόνα.

3. Όταν τελειώσετε, πατήστε το πλήκτρο **Enter** στο παράθυρο εντολών για να κλείσετε την εφαρμογή.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει μια εφαρμογή που χρησιμοποιεί την υπηρεσία αναμετάδοσης Bus υπηρεσίας, ανατρέξτε στα ακόλουθα άρθρα για να μάθετε περισσότερα σχετικά με την αναμετάδοση μηνυμάτων:

- [Αρχιτεκτονική Επισκόπηση Azure Bus υπηρεσίας](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αναμετάδοσης Bus υπηρεσίας](service-bus-dotnet-how-to-use-relay.md)

[Πύλη του Azure]: https://portal.azure.com