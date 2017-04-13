<properties 
    pageTitle="Υπηρεσία Bus όπου μεσολαβούν ανταλλαγής μηνυμάτων πρόγραμμα εκμάθησης .NET | Microsoft Azure"
    description="Όπου μεσολαβούν ανταλλαγής μηνυμάτων .NET πρόγραμμα εκμάθησης."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Πρόγραμμα εκμάθησης όπου μεσολαβούν Bus υπηρεσία ανταλλαγής μηνυμάτων .NET

Azure Bus υπηρεσία παρέχει δύο ολοκληρωμένη ανταλλαγής μηνυμάτων λύσεις – ένα, μέσω της υπηρεσίας Κεντρική "μετάδοση" εκτελείται στο cloud που υποστηρίζει μια ποικιλία διαφορετικών πρωτοκόλλων μεταφοράς και Web των υπηρεσιών προδιαγραφές, όπως SOAP, WS-*, και ΥΠΌΛΟΙΠΟ. Ο υπολογιστής-πελάτης δεν χρειάζεται μια απευθείας σύνδεση στην υπηρεσία εσωτερικής εγκατάστασης και δεν πρέπει να γνωρίζετε πού βρίσκεται η υπηρεσία και η υπηρεσία εσωτερικής εγκατάστασης δεν χρειάζονται τις θύρες εισερχομένων άνοιγμα του τείχους προστασίας.

Η δεύτερη ανταλλαγής μηνυμάτων λύση δίνει τη δυνατότητα "όπου μεσολαβούν" δυνατότητες ανταλλαγής μηνυμάτων. Αυτές μπορεί να θεωρηθεί ως ασύγχρονης ή αποσυνδεδεμένη ανταλλαγής μηνυμάτων δυνατότητες που υποστήριξη δημοσίευσης εγγραφής χρονικό αποσύνδεση και σεναρίων με χρήση της υποδομής ανταλλαγής μηνυμάτων Bus υπηρεσίας εξισορρόπησης φόρτου. Οι αποσυνδεδεμένοι επικοινωνίας έχει πολλά πλεονεκτήματα; Για παράδειγμα, προγράμματα-πελάτες και διακομιστές μπορεί να συνδεθείτε σύμφωνα με τις ανάγκες και να εκτελέσετε λειτουργίες τους με τον τρόπο ασύγχρονης.

Αυτό το πρόγραμμα εκμάθησης προορίζεται για να σας δώσει μια επισκόπηση και πρακτικής εμπειρίας με ουρές, ένα από τα βασικά στοιχεία του Bus υπηρεσίας όπου μεσολαβούν μηνυμάτων. Αφού εργάζεστε έως την ακολουθία θεμάτων σε αυτό το πρόγραμμα εκμάθησης, θα έχετε μια εφαρμογή που συμπληρώνει μια λίστα των μηνυμάτων, δημιουργεί μια ουρά και στέλνει μηνύματα στην ουρά. Τέλος, η εφαρμογή λαμβάνει και εμφανίζει τα μηνύματα από την ουρά, στη συνέχεια, εκκαθαρίζει τα τους πόρους και τις εξόδους. Για μια αντίστοιχη πρόγραμμα εκμάθησης που περιγράφει τον τρόπο για να δημιουργήσετε μια εφαρμογή που χρησιμοποιεί η μετάδοση Bus υπηρεσίας, ανατρέξτε στο θέμα η [υπηρεσία Bus αναμετάδοση μηνυμάτων πρόγραμμα εκμάθησης](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Εισαγωγή και τις προϋποθέσεις

Ουρές προσφέρουν πρώτη στο, πρώτη ανάληψη FIFO παράδοση του μηνύματος σε μία ή περισσότερες ανταγωνιστικών καταναλωτές. FIFO σημαίνει ότι τα μηνύματα συνήθως αναμένεται να λάβει και να υποβάλλονται σε επεξεργασία από το δέκτες στη σειρά χρονικό στο οποίο ήταν που τοποθετούνται στην ουρά και κάθε μήνυμα θα λάβει και υποβάλλονται σε επεξεργασία από ένα μόνο μήνυμα καταναλωτή. Ένα βασικό πλεονέκτημα της χρήσης ουρές είναι για να επιτύχετε *χρονικό αποσύνδεση* από την εφαρμογή στοιχεία: με άλλα λόγια, τους παραγωγούς και τους καταναλωτές δεν χρειάζεται να είναι αποστολή και λήψη μηνυμάτων την ίδια στιγμή, επειδή το μηνύματα αποθηκεύονται κατάταξή στην ουρά. Μια σχετική πλεονέκτημα είναι *Φόρτωση ισοστάθμιση*, που σας επιτρέπει παραγωγών και καταναλωτών για αποστολή και λήψη μηνυμάτων διαφορετικά επιτόκια.

Ακολουθούν ορισμένα βήματα διαχείρισης και προαπαιτούμενες θα πρέπει να ακολουθήσετε πριν να ξεκινήσετε το πρόγραμμα εκμάθησης. Το πρώτο είναι να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας και για να αποκτήσετε έναν αριθμό-κλειδί υπογραφής (συσχετισμών Ασφαλείας) κοινόχρηστη πρόσβαση. Ένας χώρος ονομάτων παρέχει όριο μιας εφαρμογής για κάθε εφαρμογή εκτίθεται μέσω Bus υπηρεσίας. Έναν αριθμό-κλειδί συσχετισμών Ασφαλείας δημιουργείται αυτόματα από το σύστημα όταν δημιουργείται ένα χώρο ονομάτων υπηρεσίας. Ο συνδυασμός χώρος ονομάτων υπηρεσίας και κλειδί συσχετισμών Ασφαλείας παρέχει μια πιστοποίηση με την οποία Bus υπηρεσίας, πραγματοποιεί έλεγχο ταυτότητας πρόσβαση σε μια εφαρμογή του.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας και αποκτήσετε έναν αριθμό-κλειδί συσχετισμών Ασφαλείας

Το πρώτο βήμα είναι να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας και για να αποκτήσετε έναν αριθμό-κλειδί [Υπογραφής πρόσβασης σε κοινή χρήση](service-bus-sas-overview.md) (συσχετισμών Ασφαλείας). Ένας χώρος ονομάτων παρέχει όριο μιας εφαρμογής για κάθε εφαρμογή εκτίθεται μέσω Bus υπηρεσίας. Έναν αριθμό-κλειδί συσχετισμών Ασφαλείας δημιουργείται αυτόματα από το σύστημα όταν δημιουργείται ένα χώρο ονομάτων υπηρεσίας. Ο συνδυασμός χώρος ονομάτων υπηρεσίας και κλειδί συσχετισμών Ασφαλείας παρέχει μια διαπιστευτήρια για Bus Service για τον έλεγχο ταυτότητας πρόσβαση σε μια εφαρμογή του.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Το επόμενο βήμα είναι να δημιουργήσετε ένα έργο Visual Studio και να γράφετε δύο συναρτήσεις Βοήθειας που φόρτωση μια λίστα διαχωρισμένο με κόμματα των μηνυμάτων σε ένα αντικείμενο .NET [λίστα](https://msdn.microsoft.com/library/6sh2ey19.aspx) ισχυρού τύπου [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .

### <a name="create-a-visual-studio-project"></a>Δημιουργήστε ένα έργο Visual Studio

1. Ανοίξτε το Visual Studio ως διαχειριστής κάνοντας δεξί κλικ το πρόγραμμα στο μενού "Έναρξη" και κάνοντας κλικ στην επιλογή **Εκτέλεση ως διαχειριστής**.

1. Δημιουργήστε ένα νέο έργο εφαρμογής κονσόλας. Κάντε κλικ στο μενού **αρχείο** και επιλέξτε **Δημιουργία**και, στη συνέχεια, κάντε κλικ στο **έργο**. Στο παράθυρο διαλόγου **Νέο έργο** , κάντε κλικ στην επιλογή **Visual C#** (εάν **Visual C#** δεν εμφανίζεται, αναζητήστε στην περιοχή **Άλλες γλώσσες**), κάντε κλικ στο πρότυπο **Εφαρμογής κονσόλας** και ονομάστε το **QueueSample**. Χρησιμοποιήστε την προεπιλεγμένη **θέση**. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

1. Χρησιμοποιήστε τη Διαχείριση πακέτου NuGet για να προσθέσετε τις βιβλιοθήκες Bus υπηρεσίας στο έργο σας:
    1. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **QueueSample** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.
    2. Στο παράθυρο διαλόγου **Διαχείριση πακέτων Nuget** , κάντε κλικ στην καρτέλα **Αναζήτηση** και αναζητήστε **Bus υπηρεσίας Azure**και, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**.
<br />
1. Στην Εξερεύνηση λύσεων, κάντε διπλό κλικ στο αρχείο Program.cs για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio. Αλλάξτε το όνομα χώρου ονομάτων από το προεπιλεγμένο όνομα του `QueueSample` σε `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Τροποποίηση του `using` δηλώσεις όπως φαίνεται στο ακόλουθο κώδικα.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Δημιουργήστε ένα αρχείο κειμένου με το όνομα Data.csv και αντιγράψτε το παρακάτω κείμενο διαχωρισμένο με κόμματα.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Αποθηκεύστε και κλείστε το αρχείο Data.csv και να θυμάστε τη θέση στην οποία το αποθηκεύσατε.

1. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο όνομα του έργου σας (σε αυτό το παράδειγμα, **QueueSample**), κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπάρχον στοιχείο**.

1. Αναζητήστε το αρχείο Data.csv που δημιουργήσατε στο βήμα 6. Κάντε κλικ στο αρχείο και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**. Βεβαιωθείτε ότι * *όλα τα αρχεία (*.*) ** είναι επιλεγμένο στη λίστα των τύπων αρχείων.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Δημιουργήστε μια μέθοδο που αναλύει μια λίστα των μηνυμάτων

1. Στο το `Program` κλάση πριν από την `Main()` μέθοδο, δηλώνουν δύο μεταβλητές: μία τύπου **DataTable**, ώστε να περιέχει τη λίστα των μηνυμάτων στο Data.csv. Το άλλο πρέπει να είναι του τύπου αντικείμενο λίστας, συνιστάται ιδιαίτερα πληκτρολογήσατε για να [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Αυτή είναι η λίστα των μηνυμάτων όπου μεσολαβούν που θα χρησιμοποιούν οι επόμενες βήματα στο θέμα το πρόγραμμα εκμάθησης.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Εξωτερικά `Main()`, ορίστε μια `ParseCSV()` μέθοδο που αναλύει τη λίστα των μηνυμάτων στο Data.csv και φορτώνει τα μηνύματα σε έναν πίνακα [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) , όπως φαίνεται εδώ. Η μέθοδος επιστρέφει ένα αντικείμενο **DataTable** .

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. Στο το `Main()` μέθοδο, προσθέστε μια πρόταση που καλεί το `ParseCSVFile()` μέθοδο:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Δημιουργήστε μια μέθοδο που φορτώνει τη λίστα μηνυμάτων

1. Εξωτερικά `Main()`, ορίστε μια `GenerateMessages()` μέθοδο που λαμβάνει το αντικείμενο **DataTable** που επιστρέφονται από `ParseCSVFile()` και φορτώνει τον πίνακα σε μια λίστα ισχυρού τύπου των μηνυμάτων όπου μεσολαβούν. Η μέθοδος, στη συνέχεια, επιστρέφει το αντικείμενο **λίστας** , όπως στο παρακάτω παράδειγμα. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. Στο `Main()`, αμέσως μετά την κλήση σε `ParseCSVFile()`, προσθέστε μια πρόταση που καλεί το `GenerateMessages()` μέθοδο με την τιμή επιστροφής από `ParseCSVFile()` ως όρισμα:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Αποκτήστε τα διαπιστευτήρια χρήστη

1. Πρώτα, δημιουργήστε τρεις μεταβλητές καθολικού συμβολοσειρά για τη διατήρηση αυτών των τιμών. Δηλώνουν αυτές τις μεταβλητές αμέσως μετά το προηγούμενο δηλώσεις μεταβλητών Για παράδειγμα:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Στη συνέχεια, δημιουργήστε μια συνάρτηση που αποδέχεται και αποθηκεύει το χώρος ονομάτων υπηρεσίας και το κλειδί συσχετισμών Ασφαλείας. Προσθέστε αυτήν τη μέθοδο εκτός της `Main()`. Για παράδειγμα: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. Στο `Main()`, αμέσως μετά την κλήση σε `GenerateMessages()`, προσθέστε μια πρόταση που καλεί το `CollectUserInput()` μέθοδο:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Δημιουργία της λύσης

Από το μενού **Δημιουργία** στο Visual Studio, μπορείτε να κάντε κλικ στην επιλογή **Δημιουργία λύσης** ή πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** για να επιβεβαιώσετε την ακρίβεια της εργασίας σας μέχρι στιγμής.

## <a name="create-management-credentials"></a>Δημιουργία διαπιστευτήρια διαχείρισης

Σε αυτό το βήμα, μπορείτε να ορίσετε τις λειτουργίες διαχείρισης που θα χρησιμοποιήσετε για να δημιουργήσετε κοινόχρηστη πρόσβαση διαπιστευτήρια υπογραφής (συσχετισμών Ασφαλείας) με την οποία θα επιτρέπεται η εφαρμογή σας.

1. Για σαφήνεια, αυτό το πρόγραμμα εκμάθησης τοποθετεί όλες οι λειτουργίες ουράς σε μια ξεχωριστή μέθοδο. Δημιουργία μιας ασύγχρονης `Queue()` μέθοδο σε το `Program` κλάση, μετά την `Main()` μέθοδο. Για παράδειγμα:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Το επόμενο βήμα είναι να δημιουργήσετε μια πιστοποίηση συσχετισμών Ασφαλείας χρησιμοποιώντας ένα αντικείμενο [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) . Τη μέθοδο δημιουργίας λαμβάνει το όνομα του κλειδιού συσχετισμών Ασφαλείας και την τιμή που λαμβάνονται στο το `CollectUserInput()` μέθοδο. Προσθέστε τον ακόλουθο κώδικα για την `Queue()` μέθοδο:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Δημιουργήστε ένα νέο χώρο ονομάτων αντικείμενο διαχείρισης, με ένα URI που περιέχει το όνομα χώρου ονομάτων και τα διαπιστευτήρια διαχείρισης που λαμβάνονται στο προηγούμενο βήμα, ως ορίσματα. Προσθέστε αυτόν τον κωδικό αμέσως μετά τον κωδικό που προσθέσατε στο προηγούμενο βήμα. Βεβαιωθείτε ότι για να αντικαταστήσετε `<yourNamespace>` με το όνομα του πεδίου ονομάτων υπηρεσίας:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Παράδειγμα

Σε αυτό το σημείο, τον κωδικό θα πρέπει να μοιάζει με το εξής:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Αποστολή μηνυμάτων στην ουρά

Σε αυτό το βήμα, θα δημιουργήσετε μια ουρά, στη συνέχεια, αποστολή των μηνυμάτων που περιέχονται στη λίστα των μηνυμάτων όπου μεσολαβούν στην ουρά.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Δημιουργία ουράς και αποστολή μηνυμάτων στην ουρά

1. Πρώτα, δημιουργήστε την ουρά. Για παράδειγμα, καλέστε την `myQueue`, και να δηλώνουν το απευθείας μετά τις εργασίες διαχείρισης που προσθέσατε στο το `Queue()` μέθοδο στο τελευταίο βήμα:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. Στο το `Queue()` μέθοδο, δημιουργία αντικειμένου εργοστασίου ανταλλαγής μηνυμάτων με ένα πρόσφατα δημιουργημένο υπηρεσίας Bus URI το ως όρισμα. Προσθέστε τον ακόλουθο κώδικα απευθείας μετά τις εργασίες διαχείρισης που προσθέσατε στο τελευταίο βήμα. Βεβαιωθείτε ότι για να αντικαταστήσετε `<yourNamespace>` με το όνομα του πεδίου ονομάτων υπηρεσίας:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Στη συνέχεια, δημιουργήστε το αντικείμενο ουρά χρησιμοποιώντας την κλάση [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Προσθέστε τον ακόλουθο κώδικα αμέσως μετά τον κωδικό που προσθέσατε στο τελευταίο βήμα:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Στη συνέχεια, προσθέσετε κώδικα που διέρχεται από τη λίστα των μηνυμάτων όπου μεσολαβούν που δημιουργήσατε προηγουμένως, αποστολή κάθε στην ουρά. Προσθέστε τον ακόλουθο κώδικα απευθείας μετά το `CreateQueueClient()` δήλωση στο προηγούμενο βήμα:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Λήψη μηνυμάτων από την ουρά

Σε αυτό το βήμα, μπορείτε να αποκτήσετε τη λίστα των μηνυμάτων από την ουρά που δημιουργήσατε στο προηγούμενο βήμα.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Δημιουργήστε ένα δέκτη και να λαμβάνετε μηνύματα από την ουρά

Στο το `Queue()` μέθοδος διαδοχικές προσεγγίσεις μέσα στην ουρά και να λαμβάνετε τα μηνύματα με τη μέθοδο [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) , εκτυπώσετε κάθε μήνυμα στην κονσόλα. Προσθέστε τον ακόλουθο κώδικα αμέσως μετά τον κωδικό που προσθέσατε στο προηγούμενο βήμα:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Λάβετε υπόψη ότι η `Thread.Sleep` χρησιμοποιείται μόνο για να προσομοιώσετε το μήνυμα Τοποθέτησης και μάλλον δεν θα χρειαστείτε σε μια εφαρμογή πραγματικό ανταλλαγής μηνυμάτων.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Να τερματίσετε τη μέθοδο ουρά και να εκκαθαρίσετε πόροι

Αμέσως μετά το προηγούμενο κώδικα, προσθέστε τον ακόλουθο κώδικα για να καθαρίσετε το μήνυμα εργοστασίου και ουρά πόρους:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Η κλήση της μεθόδου ουρά

Το τελευταίο βήμα είναι να προσθέσετε μια πρόταση που καλεί το `Queue()` από τις μεθόδους `Main()`. Προσθέστε την ακόλουθη γραμμή επισήμανση του κώδικα στο τέλος της Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας περιέχει την πλήρη εφαρμογή **QueueSample** .

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Δημιουργία και εκτέλεση της εφαρμογής QueueSample

Τώρα που έχετε ολοκληρώσει τα παραπάνω βήματα, μπορείτε να δημιουργήσετε και να εκτελέσετε την εφαρμογή **QueueSample** .

### <a name="build-the-queuesample-application"></a>Δημιουργία της εφαρμογής QueueSample

Στο Visual Studio, από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύσης**ή πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B**. Εάν αντιμετωπίζετε σφάλματα, βεβαιωθείτε ότι ο κώδικας είναι σωστή με βάση το παράδειγμα ολοκλήρωσης που παρουσιάζονται στο τέλος της το προηγούμενο βήμα.

## <a name="next-steps"></a>Επόμενα βήματα

Αυτό το πρόγραμμα εκμάθησης εμφάνιζε πώς να δημιουργείτε ένα πρόγραμμα-πελάτη Bus υπηρεσίας εφαρμογών και υπηρεσιών χρησιμοποιώντας τις δυνατότητες ανταλλαγής μηνυμάτων όπου μεσολαβούν Bus υπηρεσίας. Για ένα παρόμοιο πρόγραμμα εκμάθησης που χρησιμοποιεί Bus υπηρεσίας [αναμετάδοσης](service-bus-messaging-overview.md#Relayed-messaging), ανατρέξτε στο θέμα η [υπηρεσία Bus αναμετάδοση μηνυμάτων πρόγραμμα εκμάθησης](../service-bus-relay/service-bus-relay-tutorial.md).

Για να μάθετε περισσότερα σχετικά με την [Υπηρεσία Bus](https://azure.microsoft.com/services/service-bus/), ανατρέξτε στα ακόλουθα θέματα.

- [Επισκόπηση μηνυμάτων Bus υπηρεσίας](service-bus-messaging-overview.md)
- [Υπηρεσία Bus τα θεμελιώδη στοιχεία](service-bus-fundamentals-hybrid-solutions.md)
- [Αρχιτεκτονική Bus υπηρεσίας](service-bus-architecture.md)

