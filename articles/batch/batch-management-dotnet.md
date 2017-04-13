<properties
    pageTitle="Διαχείρισης πόρων με δέσμη διαχείρισης .NET λογαριασμού | Microsoft Azure"
    description="Δημιουργία, διαγραφή και τροποποίηση δέσμη Azure λογαριασμό πόρων με τη βιβλιοθήκη δέσμη διαχείρισης .NET."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Διαχείριση λογαριασμών δέσμη Azure και ορίων με δέσμη διαχείρισης .NET

> [AZURE.SELECTOR]
- [Πύλη του Azure](batch-account-create-portal.md)
- [Δέσμη διαχείρισης .NET](batch-management-dotnet.md)

Μπορείτε να μειώσετε συντήρησης επιβάρυνσης στις εφαρμογές σας δέσμη Azure χρησιμοποιώντας τη [Μαζική διαχείρισης .NET] [ api_mgmt_net] βιβλιοθήκη για την αυτοματοποίηση μαζική Δημιουργία λογαριασμού, διαγραφή, διαχείριση κλειδιών και ορίου εντοπισμού.

- **Δημιουργία και διαγραφή λογαριασμών δέσμη** μέσα σε κάθε περιοχή. Εάν, με έναν προμηθευτή ανεξάρτητη λογισμικού (ISV) για παράδειγμα, παρέχετε υπηρεσίας για τους πελάτες σας με την οποία κάθε εκχωρείται ένα ξεχωριστό λογαριασμό δέσμη για σκοπούς χρέωσης, μπορείτε να προσθέσετε δυνατότητες δημιουργίας και διαγραφή λογαριασμού στην πύλη του πελάτη σας.
- **Ανάκτηση και κλειδιά regenerate λογαριασμού** μέσω προγραμματισμού για οποιονδήποτε από τους λογαριασμούς σας δέσμης. Αυτό μπορεί να σας βοηθήσει να συμμορφώνονται με τις πολιτικές ασφαλείας που επιβάλλουν περιοδικό κατάδειξης ή τη λήξη της πλήκτρα λογαριασμού. Όταν έχετε περισσότερους από έναν λογαριασμούς δέσμη σε διάφορες περιοχές Azure, αυτοματισμού αυτής της διαδικασίας κατάδειξης αυξάνει την αποτελεσματικότητα της λύσης σας.
- **Ελέγξτε το λογαριασμό ορίων** και τραβήξτε τη δοκιμαστική έκδοση και σφάλματος κόπο από τον προσδιορισμό των λογαριασμών δέσμη έχουν τα όρια. Ελέγχοντας το λογαριασμό ορίων πριν από την έναρξη εργασιών, τη δημιουργία χώρους συγκέντρωσης ή προσθήκη κόμβους υπολογιστικών, μπορείτε να προσαρμόσετε εκ των προτέρων πού ή όταν αυτά τον υπολογισμό δημιουργούνται πόρους. Μπορείτε να καθορίσετε τους λογαριασμούς που απαιτούν ορίου αυξάνεται πριν από την ανάθεση επιπλέον πόρων σε αυτούς τους λογαριασμούς.
- **Συνδυασμός δυνατότητες από άλλες υπηρεσίες του Azure** για μια εμπειρία πλήρεις δυνατότητες διαχείρισης--με χρήση δέσμης διαχείρισης .NET, [Azure Active Directory][aad_about], και τη [Διαχείριση πόρων Azure] [ resman_overview] μαζί στην ίδια εφαρμογή. Χρησιμοποιώντας αυτές τις δυνατότητες και τους API του Yammer, μπορείτε να παρέχετε μια εμπειρία frictionless ελέγχου ταυτότητας, η δυνατότητα για τη δημιουργία και διαγραφή ομάδων πόρων και τις δυνατότητες που περιγράφονται παραπάνω για μια λύση για να ολοκληρωμένες διαχείρισης.

> [AZURE.NOTE] Ενώ σε αυτό το άρθρο εστιάζει για τη διαχείριση των λογαριασμών δέσμη, πλήκτρα και ορίων του προγραμματισμού, μπορείτε να εκτελέσετε πολλές από αυτές τις δραστηριότητες, χρησιμοποιώντας την [πύλη του Azure][azure_portal]. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία δέσμης Azure λογαριασμού με την πύλη Azure](batch-account-create-portal.md) και [τα όρια και περιορισμοί για την υπηρεσία δέσμη Azure](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Δημιουργία και διαγραφή δέσμης λογαριασμών

Όπως αναφέρθηκε, μία από τις βασικές δυνατότητες του το API διαχείρισης δέσμη είναι να δημιουργήσετε και να διαγράψετε τους λογαριασμούς δέσμη σε μια περιοχή Azure. Για να το κάνετε αυτό, χρησιμοποιήστε [BatchManagementClient.Account.CreateAsync] [ net_create] και [DeleteAsync][net_delete], ή τα σύγχρονη αντίστοιχα.

Το παρακάτω τμήμα κώδικα δημιουργεί ένα λογαριασμό, λαμβάνει το λογαριασμό που μόλις δημιουργήθηκε από την υπηρεσία δέσμη και, στη συνέχεια, διαγράφει το. Σε αυτό το τμήμα κώδικα και τα άλλα άτομα σε αυτό το άρθρο `batchManagementClient` είναι μια πλήρως προετοιμασία παρουσία της [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Εφαρμογές που χρησιμοποιούν τη βιβλιοθήκη .NET δέσμη διαχείρισης και την κλάση BatchManagementClient απαιτούν πρόσβαση **διαχειριστή της υπηρεσίας** ή **coadministrator** για τη συνδρομή στην οποία ανήκει το λογαριασμό δέσμη για διαχείριση. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Azure Active Directory](#azure-active-directory) και το [AccountManagement] [ acct_mgmt_sample] δείγμα κώδικα.

## <a name="retrieve-and-regenerate-account-keys"></a>Ανάκτηση και να δημιουργήσετε ξανά πλήκτρα λογαριασμού

Λάβετε πλήκτρα κύριας και δευτερεύουσας λογαριασμού από οποιονδήποτε λογαριασμό δέσμη μέσα τη συνδρομή σας χρησιμοποιώντας [ListKeysAsync][net_list_keys]. Μπορείτε να αναδημιουργήσετε αυτά τα πλήκτρα με τη χρήση [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Μπορείτε να δημιουργήσετε μια ροή εργασίας βελτιωμένο σύνδεσης για τις εφαρμογές σας διαχείρισης. Πρώτα, να αποκτήσετε ένα κλειδί λογαριασμού για το λογαριασμό δέσμη που θέλετε να διαχειριστείτε με [ListKeysAsync][net_list_keys]. Στη συνέχεια, χρησιμοποιήστε αυτό το κλειδί κατά την προετοιμασία της βιβλιοθήκης .NET δέσμη [BatchSharedKeyCredentials] [ net_sharedkeycred] κλάση, η οποία χρησιμοποιείται κατά την προετοιμασία [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Έλεγχος Azure συνδρομής και ορίων λογαριασμού δέσμης

Azure συνδρομές και τις μεμονωμένες Azure υπηρεσίες όπως μαζική όλα έχουν προεπιλεγμένη ορίων που περιορίζει τον αριθμό των ορισμένες οντοτήτων μέσα σε αυτά. Για την προεπιλεγμένη ορίων για Azure συνδρομές, ανατρέξτε στο θέμα [Azure συνδρομής και όρια υπηρεσίας, όρια, και τους περιορισμούς](./../azure-subscription-service-limits.md). Για την προεπιλεγμένη όρια της υπηρεσίας δέσμης, ανατρέξτε στο θέμα [όρια και περιορισμοί για την υπηρεσία δέσμη Azure](batch-quota-limit.md). Χρησιμοποιώντας τη μαζική διαχείρισης .NET βιβλιοθήκη, μπορείτε να ελέγξετε αυτά τα όρια στις εφαρμογές σας. Αυτό σας επιτρέπει να αποφάσεων εκχώρησης πριν να προσθέσετε λογαριασμούς ή τον υπολογισμό πόρους, όπως σύνολα και τον υπολογισμό κόμβους.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Επιλέξτε μια Azure συνδρομή για τα όρια λογαριασμό δέσμης

Πριν να δημιουργήσετε ένα λογαριασμό δέσμη σε μια περιοχή, μπορείτε να ελέγξετε Azure τη συνδρομή σας για να δείτε αν θα μπορείτε να προσθέσετε ένα λογαριασμό σε αυτήν την περιοχή.

Στο τμήμα κώδικα παρακάτω, χρησιμοποιούμε πρώτα [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] για να λάβετε μια συλλογή από όλους τους λογαριασμούς δέσμη που βρίσκονται μέσα σε μια συνδρομή. Αφού μας έχετε λάβει αυτήν τη συλλογή, θα σας διαπιστώσετε πόσους λογαριασμούς στην περιοχή προορισμού. Στη συνέχεια, χρησιμοποιούμε [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] για να αποκτήσετε του ορίου δέσμη λογαριασμό και τον καθορισμό πόσους λογαριασμούς (εάν υπάρχουν) μπορούν να δημιουργηθούν σε αυτήν την περιοχή.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Στο τμήμα παραπάνω, `creds` είναι μια παρουσία του [TokenCloudCredentials][azure_tokencreds]. Για να δείτε ένα παράδειγμα της δημιουργίας αυτού του αντικειμένου, ανατρέξτε στο θέμα το [AccountManagement] [ acct_mgmt_sample] δείγμα κώδικα στην GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Επιλέξτε ένα λογαριασμό δέσμη για ορίων πόρων υπολογισμού

Πριν να αυξήσετε πόρους υπολογισμού στη λύση σας δέσμη, μπορείτε να ελέγξετε για να βεβαιωθείτε ότι οι πόροι που θέλετε να δεσμεύσετε δεν υπερβαίνει τα όρια του λογαριασμού. Το τμήμα κώδικα παρακάτω, θα σας εκτυπώσετε τις πληροφορίες ορίου για το λογαριασμό δέσμης με το όνομα `mybatchaccount`. Στη δική σας εφαρμογή, μπορείτε να χρησιμοποιήσετε τις πληροφορίες για να προσδιορίσετε αν ο λογαριασμός μπορεί να διαχειριστεί τους επιπλέον πόρους που θα δημιουργηθεί.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Παρόλο που υπάρχουν ορίων προεπιλογή για συνδρομές του Azure και υπηρεσίες, πολλά από αυτά τα όρια μπορεί να είναι προκύπτουν από την έκδοση αίτησης στην [πύλη του Azure][azure_portal]. Για παράδειγμα, ανατρέξτε στο θέμα [όρια και περιορισμοί για την υπηρεσία Azure δέσμη](batch-quota-limit.md) για οδηγίες σχετικά με αύξουσα σας ορίων λογαριασμού δέσμης.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Μαζική διαχείρισης .NET, Azure AD, και διαχείριση πόρων

Όταν εργάζεστε με τη βιβλιοθήκη δέσμη διαχείρισης .NET, συνήθως χρησιμοποιείτε επίσης [Azure Active Directory] [ aad_about] (Azure AD) και τη [Διαχείριση πόρων Azure][resman_overview]. Το δείγμα αναφέρονται παρακάτω χρησιμοποιεί το project Azure Active Directory και τη διαχείριση πόρων ενώ επιδεικνύεται το API δέσμη διαχείρισης .NET.

### <a name="azure-active-directory"></a>Καταλόγου Azure Active Directory

Azure ίδια χρησιμοποιεί Azure AD για τον έλεγχο ταυτότητας των πελατών, διαχειριστές υπηρεσιών, και χρήστες του οργανισμού. Στο πλαίσιο της δέσμης διαχείρισης .NET, μπορείτε να χρησιμοποιήσετε Azure AD για τον έλεγχο ταυτότητας μια συνδρομή διαχειριστή ή διαχειριστή από κοινού. Αυτό σας επιτρέπει η βιβλιοθήκη διαχείρισης για να ερωτήματος στην υπηρεσία δέσμη και να εκτελέσετε τις λειτουργίες που περιγράφονται σε αυτό το άρθρο.

Στο δείγμα έργο που αναφέρονται κάτω από το Azure [Βιβλιοθήκη ελέγχου ταυτότητας Active Directory] [ aad_adal] (ADAL) χρησιμοποιείται για να γίνεται ερώτηση στο χρήστη για τα διαπιστευτήρια της Microsoft. Κατά την υπηρεσία διαχειριστή ή coadministrator διαπιστευτήρια, η εφαρμογή μπορεί να ερωτήματος Azure για μια λίστα των συνδρομών--και μπορεί να δημιουργία και διαγραφή ομάδες πόρων και λογαριασμούς δέσμης.

### <a name="resource-manager"></a>Διαχείριση πόρων

Όταν δημιουργείτε λογαριασμούς δέσμη με τη βιβλιοθήκη δέσμη διαχείρισης .NET, που θα συνήθως δημιουργούν τους μέσα σε μια [ομάδα πόρων][resman_overview]. Μπορείτε να δημιουργήσετε την ομάδα των πόρων μέσω προγραμματισμού, χρησιμοποιώντας το [ResourceManagementClient] [ resman_client] κλάση στη [Διαχείριση πόρων .NET] [ resman_api] βιβλιοθήκη. Ή μπορείτε να προσθέσετε ένα λογαριασμό σε μια υπάρχουσα ομάδα πόρων που δημιουργήσατε προηγουμένως με τη χρήση του [Azure πύλη][azure_portal].

## <a name="sample-project-on-github"></a>Δείγμα έργου σε GitHub

Για να δείτε .NET διαχείρισης δέσμη ενεργειών, ανατρέξτε στο θέμα το [AccountManagment] [ acct_mgmt_sample] δείγμα έργου σε GitHub. Αυτή η εφαρμογή κονσόλας εμφανίζει τη δημιουργία και χρήση της [BatchManagementClient] [ net_mgmt_client] και [ResourceManagementClient][resman_client]. Παρουσιάζει επίσης η χρήση της Azure [Βιβλιοθήκη ελέγχου ταυτότητας Active Directory] [ aad_adal] (ADAL), που απαιτείται από δύο προγράμματα-πελάτες.

Για να εκτελέσετε το δείγμα εφαρμογής με επιτυχία, πρέπει πρώτα να καταχωρήσετε το με Azure AD χρησιμοποιώντας την πύλη του Azure. Ακολουθήστε τα βήματα στην ενότητα [Προσθήκη μιας εφαρμογής](../active-directory/active-directory-integrating-applications.md#adding-an-application) στις [εφαρμογές ολοκλήρωση με Azure Active Directory] [ aad_integrate] για να καταχωρήσετε το δείγμα εφαρμογής εντός το δικό σας λογαριασμό του προεπιλεγμένου καταλόγου. Φροντίστε να επιλέξετε **Εγγενή εφαρμογή προγράμματος-πελάτη** για τον τύπο της εφαρμογής και μπορείτε να καθορίσετε οποιαδήποτε έγκυρη URI (όπως είναι οι `http://myaccountmanagementsample`) για την **Ανακατεύθυνση URI**--δεν χρειάζεται να είναι ένα πραγματικό τελικό σημείο.

Αφού προσθέσετε την εφαρμογή σας, πληρεξούσιου το δικαίωμα **Πρόσβασης Azure υπηρεσίας διαχείρισης ως εταιρεία** στην εφαρμογή *Windows Azure υπηρεσίας διαχείρισης API* στις ρυθμίσεις της εφαρμογής στην πύλη:

![Δικαιώματα εφαρμογής στην πύλη του Azure][2]

> [AZURE.TIP] Εάν **API διαχείρισης της υπηρεσίας Windows Azure** δεν εμφανίζεται στην περιοχή *δικαιώματα σε άλλες εφαρμογές*, κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής**, επιλέξτε **API διαχείρισης της υπηρεσίας Windows Azure**, στη συνέχεια, κάντε κλικ στο κουμπί σημάδι ελέγχου. Στη συνέχεια, ανάθεση δικαιωμάτων όπως καθορίζεται παραπάνω.

Αφού προσθέσετε την εφαρμογή όπως περιγράφεται παραπάνω, ενημέρωση `Program.cs` στο το [AccountManagment] [ acct_mgmt_sample] δείγμα έργου με ανακατεύθυνση URI της εφαρμογής σας και το αναγνωριστικό υπολογιστή-πελάτη. Μπορείτε να βρείτε αυτές τις τιμές στην καρτέλα **Ρύθμιση παραμέτρων** της εφαρμογής σας:

![Ρύθμιση παραμέτρων εφαρμογής στην πύλη του Azure][3]

Το [AccountManagment] [ acct_mgmt_sample] δείγμα εφαρμογής παρουσιάζει τις ακόλουθες λειτουργίες:

1. Απόκτηση ενός διακριτικού ασφαλείας από το Azure AD με χρήση του [ADAL][aad_adal]. Εάν ο χρήστης δεν έχει ήδη πραγματοποιήσει είσοδο, θα σας ζητηθεί για τα διαπιστευτήριά τους Azure.
2. Με τη χρήση του διακριτικού ασφαλείας που λαμβάνονται από το Azure AD, δημιουργήστε μια [SubscriptionClient] [ resman_subclient] ερώτημα Azure για μια λίστα των συνδρομών που σχετίζεται με το λογαριασμό. Αυτό σας επιτρέπει να βρεθούν επιλογής μιας εγγραφής εάν πολλών.
3. Δημιουργήστε ένα αντικείμενο διαπιστευτήρια που σχετίζεται με την επιλεγμένη εγγραφή.
4. Δημιουργία [ResourceManagementClient] [ resman_client] χρησιμοποιώντας τα διαπιστευτήρια.
5. Χρήση [ResourceManagementClient] [ resman_client] για να δημιουργήσετε μια ομάδα πόρων.
6. Χρήση [BatchManagementClient] [ net_mgmt_client] για να εκτελέσετε διάφορες εργασίες λογαριασμό δέσμη:
  - Δημιουργήστε ένα λογαριασμό δέσμη στη νέα ομάδα πόρων.
  - Λάβετε το λογαριασμό που μόλις δημιουργήθηκε από την υπηρεσία δέσμης.
  - Εκτύπωση τα πλήκτρα λογαριασμού για τον νέο λογαριασμό.
  - Αναδημιουργήσετε ένα νέο πρωτεύον κλειδί για το λογαριασμό.
  - Εκτύπωση των πληροφοριών ορίου για το λογαριασμό.
  - Εκτύπωση των πληροφοριών ορίου για τη συνδρομή.
  - Εκτύπωση όλοι οι λογαριασμοί εντός της συνδρομής.
  - Διαγράψτε το λογαριασμό που μόλις δημιουργήθηκε.
7. Διαγραφή ομάδας πόρων.

Πριν από τη διαγραφή που έχουν δημιουργηθεί πρόσφατα δέσμη λογαριασμού και πόρων ομάδα, μπορείτε να ελέγξετε και τα δύο στην [πύλη του Azure][azure_portal]:

![Azure πύλη που εμφανίζει την ομάδα πόρων και δέσμη λογαριασμού][1]
<br />
*Azure πύλη Εμφανίζει νέας ομάδας πόρων και δέσμη λογαριασμού*

[aad_about]: ../active-directory/active-directory-whatis.md "Τι είναι το Azure Active Directory;"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Έλεγχος ταυτότητας σενάρια για Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Ενσωμάτωση εφαρμογών με το Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
