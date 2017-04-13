<properties
   pageTitle="Σημειώσεις έκδοσης θάλαμο .NET 2.x API βασικές | Microsoft Azure"
   description="Προγραμματιστές .NET θα χρησιμοποιήσετε αυτό το API στον κώδικα για Azure θάλαμο αριθμού-κλειδιού"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure κλειδιού θάλαμο .NET 2.0 - σημειώσεις έκδοσης και οδηγού μετεγκατάστασης

Είναι τις παρακάτω οδηγίες και τις σημειώσεις για τους προγραμματιστές εργασία με το .NET Azure κλειδί θάλαμο / C# βιβλιοθήκη. Στο πλαίσιο αλλαγή από την έκδοση 1.0 στην έκδοση 2.0, έναν αριθμό ενημερώσεις έχουν γίνει θα απαιτούν εργασίας μετεγκατάστασης στον κώδικά σας για να επωφεληθείτε από τις λειτουργικές βελτιώσεις και προσθήκες όπως θάλαμο κλειδί πιστοποιητικά υποστήριξη των δυνατοτήτων.

Υποστήριξη πιστοποιητικά θάλαμο κλειδί παρέχει για τη διαχείριση των σας x509 πιστοποιητικά και τις ακόλουθες συμπεριφορές:  

-   Επιτρέπει στον κάτοχο ένα πιστοποιητικό για να δημιουργήσετε ένα πιστοποιητικό μέσω ενός αριθμού-κλειδιού θάλαμο διαδικασία δημιουργίας ή μέσω της εισαγωγής από ένα υπάρχον πιστοποιητικό. Περιλαμβάνονται και τα δύο αυτο-υπογεγραμμένο και αρχή έκδοσης πιστοποιητικών που δημιουργούνται από πιστοποιητικά.

- Επιτρέπει στον κάτοχο ένα πλήκτρο θάλαμο πιστοποιητικό για την υλοποίηση ασφαλούς αποθήκευσης και τη διαχείριση των X509 πιστοποιητικά χωρίς αλληλεπίδραση με υλικό ιδιωτικού κλειδιού.  

-   Επιτρέπει στον κάτοχο ένα πιστοποιητικό για να δημιουργήσετε μια πολιτική που κατευθύνει θάλαμο αριθμού-κλειδιού για τη διαχείριση του κύκλου ζωής ενός πιστοποιητικού.  

-   Επιτρέπει στους κατόχους πιστοποιητικό για να παρέχετε πληροφορίες επικοινωνίας για ειδοποίηση σχετικά με τα συμβάντα κύκλου ζωής της λήξη και την ανανέωση του πιστοποιητικού.  

-   Υποστηρίζει την αυτόματη ανανέωση με επιλεγμένο εκδότες - κλειδί θάλαμο συνεργάτη X509 πιστοποιητικού υπηρεσίες παροχής / πιστοποιητικού αρχές.
    - ΣΗΜΕΊΩΣΗ - μη συνεργαστεί υπηρεσίες παροχής/αρχές επίσης τη δυνατότητα, αλλά, δεν υποστηρίζει τη δυνατότητα αυτόματης ανανέωσης.


## <a name="net-support"></a>Υποστήριξη .NET
- **.NET 4.0** δεν υποστηρίζεται από την έκδοση 2.0 του .NET θάλαμο κλειδί Azure / C# βιβλιοθήκης
- **.NET πυρήνα** υποστηρίζεται από την έκδοση 2.0 του .NET θάλαμο κλειδί Azure / C# βιβλιοθήκης

## <a name="namespaces"></a>Οι χώροι ονομάτων
- Ο χώρος ονομάτων για **μοντέλα** αλλάζει από **Microsoft.Azure.KeyVault** **Microsoft.Azure.KeyVault.Models**.
- Ο χώρος ονομάτων **Microsoft.Azure.KeyVault.Internal** έχει καταργηθεί.
- Ο χώρος ονομάτων εξαρτήσεις Azure SDK αλλάζουν από **Hyak.Common** και **Hyak.Common.Internals** σε **Microsoft.Rest** και **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Ο τύπος αλλάζει
- *Μυστικό* αλλάξει σε *SecretBundle*
- *Λεξικό* αλλάξει *IDictionary*
- *Λίστα<T>, συμβολοσειράς []* αλλάξει σε *IList<T> *
- *NextList* αλλάξει σε *NextPageLink*


## <a name="return-types"></a>Τύποι επιστροφής
- Θα επιστρέψει **KeyList** και **SecretList** *IPage<T> * αντί για *ListKeysResponseMessage*
- Το που δημιουργήθηκε **BackupKeyAsync** θα επιστρέψει *BackupKeyResult* , το οποίο περιέχει *τιμή* (blob αντίγραφο ασφαλείας). Πριν από τη μέθοδο έγινε συσκευασία και μόνο την τιμή επιστροφής.

## <a name="exceptions"></a>Εξαιρέσεις
- *KeyVaultClientException* μετατρέπεται σε *KeyVaultErrorException*
- Το σφάλμα υπηρεσίας έχει αλλάξει από την εξαίρεση *. Σφάλμα* να *εξαίρεση. Body.Error.Message*.
- Κατάργηση επιπλέον πληροφορίες από το μήνυμα σφάλματος για **[JsonExtensionData]**.

## <a name="constructors"></a>Κατασκευές
- Αντί να αποδεχτεί μια *HttpClient* ως όρισμα κατασκευής, η κατασκευή δέχεται μόνο *HttpClientHandler* ή *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Πακέτων που έχουν ληφθεί  
Όταν ένα πρόγραμμα-πελάτη επεξεργάζεται μια εξάρτηση από θάλαμο κλειδί έχουν ληφθεί τα εξής
#### <a name="previous-package-list"></a>Προηγούμενη λίστα πακέτου
- έκδοση συσκευασία id="Hyak.Common" = "1.0.2" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Azure.Common" = "2.0.4" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Τρέχουσα λίστα πακέτου
- έκδοση συσκευασία id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
- έκδοση συσκευασία id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Αλλαγές τάξης

- Κλάση **UnixEpoch** έχει καταργηθεί
- Κλάση **Base64UrlConverter** έχει μετονομαστεί σε **Base64UrlJsonConverter**

## <a name="other-changes"></a>Άλλες αλλαγές

- Υποστήριξη για τη ρύθμιση παραμέτρων της πολιτικής "Επανάληψη" λειτουργία KV σε μεταβατικές αποτυχίες έχει προστεθεί σε αυτή την έκδοση του API.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Για τις εργασίες που επιστρέφεται ένα *θάλαμο*, ο επιστρεφόμενος τύπος έχει μια κλάση που περιέχονται σε μια ιδιότητα θάλαμο. Ο επιστρεφόμενος τύπος είναι τώρα *θάλαμο*.
- *PermissionsToKeys* και *PermissionsToSecrets* είναι πλέον *Permissions.Keys* και *Permissions.Secrets*
- Ορισμένες από τις αλλαγές τύπους επιστροφής ισχύουν για το στοιχείο ελέγχου-επίπεδο καθώς και.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Το πακέτο είναι εσφαλμένος προς τα επάνω για να **Microsoft.Azure.KeyVault.Extensions** και **Microsoft.Azure.KeyVault.Cryptography** για τις λειτουργίες κρυπτογράφησης.
