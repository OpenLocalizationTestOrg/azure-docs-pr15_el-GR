<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε τη μετάδοση Bus υπηρεσίας με .NET | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αναμετάδοσης Azure Bus υπηρεσίας για να συνδέσετε δύο εφαρμογές που φιλοξενούνται σε διαφορετικές θέσεις."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αναμετάδοσης Bus υπηρεσίας Azure

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης της υπηρεσίας αναμετάδοσης Bus υπηρεσίας. Τα δείγματα είναι γραμμένες σε C# και χρησιμοποιήστε σας το Windows Communication Foundation (WCF) API με επεκτάσεις που περιέχονται στην υπηρεσία Bus συγκρότησης. Για περισσότερες πληροφορίες σχετικά με τη μετάδοση Bus υπηρεσίας, δείτε την Επισκόπηση [υπηρεσίας Bus αναμετάδοση μηνυμάτων](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Τι είναι η μετάδοση Bus υπηρεσία;

Στην υπηρεσία [υπηρεσία Bus *μεταγωγής* ](service-bus-relay-overview.md) σάς επιτρέπει να δημιουργήσετε υβριδική εφαρμογές που εκτελούνται σε ένα κέντρο δεδομένων του Azure και τις δικές σας εταιρικό περιβάλλον εσωτερικής εγκατάστασης. Η μετάδοση Bus υπηρεσίας διευκολύνει αυτό, δίνοντάς σας για την έκθεση με ασφάλεια τις υπηρεσίες Windows Communication Foundation (WCF) που βρίσκονται μέσα σε ένα εταιρικό εταιρικό δίκτυο στο cloud δημόσια, χωρίς να χρειάζεται να ανοίξετε σύνδεσης στο τείχος προστασίας ή ενοχλητικές αλλαγές στην υποδομή ένα εταιρικό δίκτυο.

![Μετάδοση έννοιες](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Η μετάδοση Bus υπηρεσία σας δίνει τη δυνατότητα στις υπηρεσίες WCF host μέσα σε υπάρχον περιβάλλον για μεγάλες επιχειρήσεις. Μπορείτε, στη συνέχεια, να αναθέσετε ακρόαση για εισερχόμενες περιόδων λειτουργίας και προσκλήσεις σε αυτές τις υπηρεσίες WCF στην υπηρεσία υπηρεσία Bus εκτελούνται μέσα σε Azure. Αυτό σας επιτρέπει να εμφανίσετε αυτές τις υπηρεσίες εφαρμογών κώδικας που εκτελείται στο Azure ή σε κινητές συσκευές εργαζόμενους ή περιβάλλοντα extranet συνεργάτη. Υπηρεσία Bus σάς επιτρέπει να με ασφάλεια στοιχείου ελέγχου που μπορούν να έχουν πρόσβαση σε αυτές τις υπηρεσίες σε λεπτομερή επίπεδο. Παρέχει μια ισχυρή και ασφαλή τρόπο για να εμφανίσετε λειτουργία της εφαρμογής και τα δεδομένα από τις υπάρχουσες λύσεις για μεγάλες επιχειρήσεις και να επωφεληθείτε από την από το cloud.

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη μετάδοση Bus υπηρεσίας για να δημιουργήσετε μια υπηρεσία web WCF που εκτίθεται χρησιμοποιώντας μια σύνδεση καναλιού TCP, που υλοποιεί ασφαλούς συνομιλίας ανάμεσα σε δύο μέρη.

## <a name="create-a-service-namespace"></a>Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας

Για να ξεκινήσετε να χρησιμοποιείτε τη μετάδοση Bus υπηρεσίας στο Azure, πρέπει πρώτα να δημιουργήσετε ένα χώρο ονομάτων. Ένα χώρο ονομάτων παρέχει ένα κοντέινερ πεδίου για την προσθήκη διεύθυνσης σε πόρους Bus υπηρεσίας στην εφαρμογή σας.

Για να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Λήψη του πακέτου NuGet Bus υπηρεσίας

Το [πακέτο NuGet Bus υπηρεσίας](https://www.nuget.org/packages/WindowsAzure.ServiceBus) είναι ο ευκολότερος τρόπος για να λάβετε το API Bus υπηρεσίας και για να ρυθμίσετε την εφαρμογή σας με όλες τις εξαρτήσεις Bus υπηρεσίας. Για να εγκαταστήσετε το πακέτο NuGet στο έργο σας, κάντε τα εξής:

1.  Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ σε **αναφορές**και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.
2.  Αναζήτηση για "Υπηρεσία Bus" και επιλέξτε το στοιχείο **Microsoft Azure Service Bus** . Κάντε κλικ στην επιλογή **εγκατάσταση** για να ολοκληρώσετε την εγκατάσταση και, στη συνέχεια, κλείστε το παράθυρο διαλόγου παρακάτω:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Χρήση υπηρεσίας Bus για την έκθεση και κατανάλωση μια υπηρεσία web SOAP με TCP

Για να εμφανίσετε μια υπάρχουσα υπηρεσία web WCF SOAP για εξωτερική κατανάλωση, πρέπει να κάνετε αλλαγές στις συνδέσεις υπηρεσίας και διευθύνσεις. Αυτό μπορεί να απαιτεί αλλαγές στο αρχείο ρύθμισης παραμέτρων ή αυτό θα μπορούσε να απαιτούν αλλαγές κώδικα, ανάλογα με το πώς να ρυθμίσετε και να ρυθμίσετε τις υπηρεσίες WCF. Σημειώστε ότι WCF σας επιτρέπει να έχετε πολλές τελικά σημεία δικτύου πάνω από την ίδια υπηρεσία, ώστε να μπορείτε να διατηρήσετε την υπάρχουσα εσωτερικό τελικά σημεία κατά την προσθήκη τελικά σημεία Bus υπηρεσία για την εξωτερική πρόσβαση την ίδια στιγμή.

Σε αυτήν την εργασία, θα δημιουργήσετε μια απλή υπηρεσία WCF και να προσθέσετε μια υπηρεσία Bus παρακολούθηση σε αυτό. Άσκηση αυτή προϋποθέτει κάποια εξοικείωση με το Visual Studio και, επομένως, δεν θα καθοδηγήσουν όλες τις λεπτομέρειες της δημιουργίας ενός έργου. Αντί για αυτό, εστιάζει στην τον κώδικα.

Προτού ξεκινήσετε αυτά τα βήματα, ολοκληρώστε την ακόλουθη διαδικασία για να ρυθμίσετε το περιβάλλον σας:

1.  Μέσα σε Visual Studio, δημιουργία εφαρμογής κονσόλας που περιέχει δύο έργα, "Πελάτης" και "Υπηρεσίας", μέσα στη λύση.
2.  Προσθέστε το πακέτο του Microsoft Azure Service Bus NuGet και τα δύο έργα. Αυτό το πακέτο προσθέτει όλες τις απαραίτητες συγκρότησης αναφορές για τα έργα σας.

### <a name="how-to-create-the-service"></a>Πώς μπορείτε να δημιουργήσετε την υπηρεσία

Πρώτα, δημιουργήστε την ίδια την υπηρεσία. Οποιαδήποτε υπηρεσία WCF αποτελείται από τουλάχιστον τρεις διαφορετικές κατηγορίες:

-   Ορισμός μιας σύμβασης που περιγράφει τι μηνυμάτων που ανταλλάσσονται και τι είναι οι λειτουργίες για να είναι δυνατή.
-   Εφαρμογή της εν λόγω σύμβασης.
-   Κεντρικός υπολογιστής που φιλοξενεί την υπηρεσία WCF και εκθέτει διάφορες τελικά σημεία.

Τα παραδείγματα κώδικα σε αυτήν την ενότητα διεύθυνση κάθε ένα από αυτά τα στοιχεία.

Η σύμβαση καθορίζει μία λειτουργία, `AddNumbers`, που προσθέτει δύο αριθμών και επιστρέφει το αποτέλεσμα. Το `IProblemSolverChannel` περιβάλλοντος εργασίας ενεργοποιεί το πρόγραμμα-πελάτη για να διαχειριστείτε πιο εύκολα τη διάρκεια ζωής του διακομιστή μεσολάβησης. Δημιουργία αυτή η διασύνδεση θεωρείται βέλτιστη πρακτική. Είναι καλή ιδέα να τοποθετήσετε αυτόν τον ορισμό σύμβασης σε ένα ξεχωριστό αρχείο, ώστε να μπορείτε να αναφέρετε αυτό το αρχείο από τα έργα σας "Πελάτης" και "Υπηρεσίας", αλλά μπορείτε επίσης να αντιγράψετε τον κώδικα στο και τα δύο έργα.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Με τη σύμβαση στη θέση, η εφαρμογή είναι trivial.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Ρύθμιση παραμέτρων κεντρικού υπολογιστή μιας υπηρεσίας μέσω προγραμματισμού

Με τη σύμβαση και την εφαρμογή στη θέση, μπορείτε να φιλοξενήσετε τώρα την υπηρεσία. Φιλοξενία παρουσιάζεται μέσα σε ένα αντικείμενο [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) , το οποίο αναλαμβάνει τη διαχείριση των παρουσιών της υπηρεσίας και φιλοξενεί τα τελικά σημεία που κάνουν ακρόαση για μηνύματα. Ο ακόλουθος κώδικας ρυθμίζει τις παραμέτρους της υπηρεσίας με μια κανονική τοπικό τελικό σημείο και ένα τελικό σημείο Bus υπηρεσία για την απεικόνιση της εμφάνισης, σε παράθεση, εσωτερικές και εξωτερικές τελικά σημεία. Αντικαταστήστε το συμβολοσειρά *χώρο ονομάτων* με το όνομα του χώρου ονομάτων και *yourKey* με το κλειδί συσχετισμών Ασφαλείας που λάβατε στο προηγούμενο βήμα ρύθμισης.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

Στο παράδειγμα, μπορείτε να δημιουργήσετε δύο τελικά σημεία που είναι σχετικά με την ίδια εφαρμογή σύμβασης. Ένα είναι τοπική και μία προβάλλεται μέσω Bus υπηρεσίας. Οι βασικές διαφορές μεταξύ τους είναι τις συνδέσεις; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) για αυτήν από την τοπική και [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) για το τελικό σημείο Bus υπηρεσίας και τις διευθύνσεις. Το τοπικό τελικό σημείο έχει μια διεύθυνση τοπικού δικτύου με διακριτές θύρα. Το τελικό σημείο υπηρεσίας Bus έχει μια διεύθυνση τελικού σημείου αποτελούνται από τη συμβολοσειρά `sb`, το όνομα του χώρου ονομάτων και η διαδρομή "Επίλυση". Το αποτέλεσμα είναι το URI `sb://[serviceNamespace].servicebus.windows.net/solver`, προσδιορίζοντας τελικού σημείου υπηρεσίας ως ένα τελικό σημείο υπηρεσίας Bus TCP με πλήρως προσδιορισμένο όνομα εξωτερικού DNS. Εάν τοποθετήσετε τον κώδικα αντικαθιστώντας τα σύμβολα κράτησης θέσης σε το `Main` συνάρτηση της εφαρμογής **υπηρεσίας** , θα έχετε μια λειτουργική υπηρεσία. Εάν θέλετε την υπηρεσία για να ακούσετε αποκλειστικά στην υπηρεσία Bus, καταργήστε τη δήλωση τοπικό τελικό σημείο.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Ρύθμιση παραμέτρων κεντρικού υπολογιστή μιας υπηρεσίας στο αρχείο App.config

Μπορείτε επίσης να ρυθμίσετε τον κεντρικό υπολογιστή χρησιμοποιώντας το αρχείο App.config. Η υπηρεσία φιλοξενίας κώδικα σε αυτήν την περίπτωση εμφανίζεται στο επόμενο παράδειγμα.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Μετακίνηση των ορισμών τελικό σημείο στο αρχείο App.config. Το πακέτο NuGet προστεθεί ήδη μια περιοχή ορισμών στο αρχείο App.config, το οποίο είναι οι επεκτάσεις απαιτούμενη ρύθμιση παραμέτρων για Bus υπηρεσίας. Το παρακάτω παράδειγμα, η οποία είναι το ακριβές αντίστοιχο του προηγούμενου κώδικα, θα πρέπει να εμφανίζεται ακριβώς κάτω από το στοιχείο **system.serviceModel** . Αυτό το παράδειγμα κώδικα προϋποθέτει ότι το χώρο ονομάτων έργου C# ονομάζεται **υπηρεσίας**.
Αντικαταστήστε τα σύμβολα κράτησης θέσης με το χώρο ονομάτων υπηρεσίας Bus υπηρεσίας και το κλειδί συσχετισμών Ασφαλείας.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Αφού κάνετε αυτές τις αλλαγές, την εκκίνηση της υπηρεσίας όπως ήταν πριν, αλλά με δύο ζωντανή τελικά σημεία: ένα τοπικό και μία ακρόαση στο cloud.

### <a name="create-the-client"></a>Δημιουργία του προγράμματος-πελάτη

#### <a name="configure-a-client-programmatically"></a>Ρύθμιση παραμέτρων ενός προγράμματος-πελάτη μέσω προγραμματισμού

Για την εκμετάλλευση της υπηρεσίας, μπορείτε να δημιουργήσετε ένα πρόγραμμα-πελάτη WCF χρησιμοποιώντας ένα αντικείμενο [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) . Υπηρεσία Bus χρησιμοποιεί ένα μοντέλο ασφάλειας που βασίζεται σε διακριτικό υλοποιηθεί με τη χρήση συσχετισμών Ασφαλείας. Η κλάση [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) αντιπροσωπεύει μια υπηρεσία παροχής διακριτικού ασφαλείας με ενσωματωμένα εργοστασίου μεθόδους που επιστρέφουν ορισμένες υπηρεσίες παροχής γνωστές διακριτικού. Το παρακάτω παράδειγμα χρησιμοποιεί τη μέθοδο [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) για να χειριστείτε την απόκτηση του κατάλληλο διακριτικού συσχετισμών Ασφαλείας. Το όνομα και το κλειδί είναι εκείνα που λαμβάνονται από την πύλη, όπως περιγράφεται στην προηγούμενη ενότητα.

Πρώτα, αναφορά ή να αντιγράψετε το `IProblemSolver` σύμβαση κώδικα από την υπηρεσία στο έργο σας προγράμματος-πελάτη.

Στη συνέχεια, αντικαταστήστε τον κώδικα στο το `Main` τη μέθοδο του προγράμματος-πελάτη, αντικαθιστώντας ξανά το κείμενο κράτησης θέσης με το χώρο ονομάτων Bus υπηρεσίας και το κλειδί συσχετισμών Ασφαλείας.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Τώρα μπορείτε να δημιουργήσετε το πρόγραμμα-πελάτη και της υπηρεσίας, εκτελέστε τις (εκτέλεση πρώτα την υπηρεσία) και ο υπολογιστής-πελάτης καλέσει την υπηρεσία και εκτυπώνει **9**. Μπορείτε να εκτελέσετε το πρόγραμμα-πελάτη και διακομιστή σε διαφορετικούς υπολογιστές, ακόμη και σε δίκτυα και επικοινωνία θα εξακολουθούν να λειτουργούν. Ο κωδικός προγράμματος-πελάτη επίσης να εκτελέσετε στο cloud ή τοπικά.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Ρύθμιση παραμέτρων ενός προγράμματος-πελάτη στο αρχείο App.config

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους του προγράμματος-πελάτη με χρήση του αρχείου App.config.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Μετακίνηση των ορισμών τελικό σημείο στο αρχείο App.config. Το παρακάτω παράδειγμα, που είναι η ίδια με τον κώδικα που αναφέρθηκε προηγουμένως, θα πρέπει να εμφανίζεται ακριβώς κάτω από το στοιχείο **system.serviceModel** . Εδώ, όπως και πριν, πρέπει να αντικαταστήσετε τους χαρακτήρες κράτησης θέσης με το χώρο ονομάτων Bus υπηρεσίας και κλειδί συσχετισμών Ασφαλείας.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία για την υπηρεσία αναμετάδοσης Bus υπηρεσίας, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

- [Υπηρεσία Bus αναμετάδοση Επισκόπηση ανταλλαγής μηνυμάτων](service-bus-relay-overview.md)
- [Αρχιτεκτονική Επισκόπηση Azure Bus υπηρεσίας](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Λήψη δειγμάτων Bus υπηρεσίας από το [Azure δείγματα][] ή δείτε την [Επισκόπηση των δειγμάτων Bus υπηρεσίας][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Δείγματα Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Επισκόπηση των δειγμάτων Bus υπηρεσίας]: ../service-bus-messaging/service-bus-samples.md