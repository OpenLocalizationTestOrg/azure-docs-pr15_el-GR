<properties
   pageTitle="Σενάριο εφαρμογή λογικής: Δημιουργήστε ένα έναυσμα Azure συναρτήσεις υπηρεσίας Bus | Microsoft Azure"
   description="Χρησιμοποιήστε συναρτήσεις Azure για να δημιουργήσετε ένα έναυσμα Bus υπηρεσίας για μια εφαρμογή λογικής"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Σενάριο εφαρμογή λογικής: Δημιουργήστε ένα έναυσμα Bus υπηρεσίας Azure χρησιμοποιώντας συναρτήσεις Azure

Μπορείτε να χρησιμοποιήσετε συναρτήσεις Azure για να δημιουργήσετε ένα έναυσμα για μια εφαρμογή λογικής όταν πρέπει να αναπτύξετε μια μεγάλη διάρκεια εκτέλεσης ακρόασης ή εργασιών. Για παράδειγμα, μπορείτε να δημιουργήσετε μια συνάρτηση που θα κάνουν ακρόαση σε μια ουρά και, στη συνέχεια, fire αμέσως μια εφαρμογή λογικής ως έναυσμα push.

## <a name="build-the-logic-app"></a>Δημιουργία της εφαρμογής λογικής

Σε αυτό το παράδειγμα, έχετε μια συνάρτηση που εκτελείται για κάθε εφαρμογή λογικής που πρέπει να ενεργοποιηθεί. Πρώτα, δημιουργήστε μια εφαρμογή της λογικής που έχει ένα έναυσμα αίτηση HTTP. Η λειτουργία καλεί αυτό το τελικό σημείο κάθε φορά που λαμβάνετε ένα μήνυμα ουράς.  

1. Δημιουργία νέας εφαρμογής λογικής; Επιλέξτε το έναυσμα **λήψη εγχειρίδιο - όταν μια αίτηση HTTP** .  
   Προαιρετικά, μπορείτε να καθορίσετε ένα σχήμα JSON για χρήση με το μήνυμα ουρά με χρήση του εργαλείου όπως [jsonschema.net](http://jsonschema.net). Επικόλληση του σχήματος στο έναυσμα. Αυτό σας βοηθά στη σχεδίαση Κατανόηση του σχήματος των δεδομένων και πολλά άλλα εύκολα ροής ιδιοτήτων μέσω της ροής εργασίας.
1. Προσθέστε τυχόν πρόσθετες ενέργειες που θέλετε να εμφανίζεται μετά τη λήψη ενός μηνύματος ουρά. Για παράδειγμα, στείλτε ένα μήνυμα ηλεκτρονικού ταχυδρομείου μέσω του Office 365.  
1. Αποθήκευση της εφαρμογής λογική για τη δημιουργία της διεύθυνσης URL επιστροφής κλήσης για το έναυσμα αυτής της εφαρμογής λογικής. Η διεύθυνση URL που εμφανίζεται στην κάρτα έναυσμα.

![Η επιστροφή κλήσης διεύθυνση URL που εμφανίζεται στην κάρτα εναυσμάτων][1]

## <a name="build-the-function"></a>Δημιουργία της συνάρτησης

Στη συνέχεια, πρέπει να δημιουργήσετε μια συνάρτηση που θα λειτουργεί ως το έναυσμα και να ακούσετε ουρά.

1. Στην [πύλη του Azure συναρτήσεις](https://functions.azure.com/signin), επιλέξτε **Νέα συνάρτηση**, και, στη συνέχεια, επιλέξτε το πρότυπο **ServiceBusQueueTrigger - C#** .

    ![Azure πύλη συναρτήσεις][2]

2. Ρύθμιση παραμέτρων της σύνδεσης με την υπηρεσία Bus ουρά (που θα χρησιμοποιήσει το SDK Azure Service Bus `OnMessageReceive()` ακρόασης).
3. Γράψτε μια απλή συνάρτηση για να καλέσετε το τελικό σημείο εφαρμογής λογικής (που δημιουργήθηκε παραπάνω), χρησιμοποιώντας το μήνυμα ουρά ως ένα έναυσμα. Ακολουθεί ένα παράδειγμα πλήρους μιας συνάρτησης. Το παράδειγμα χρησιμοποιεί μια `application/json` μήνυμα τύπο περιεχομένου, αλλά μπορείτε να αλλάξετε αυτήν την επιλογή εάν είναι απαραίτητο.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Για να ελέγξετε, προσθέστε ένα μήνυμα ουρά μέσω ενός εργαλείου όπως [Explorer Bus υπηρεσίας](https://github.com/paolosalvatori/ServiceBusExplorer). Δείτε την εφαρμογή λογικής fire αμέσως μετά τη συνάρτηση λαμβάνει το μήνυμα.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
