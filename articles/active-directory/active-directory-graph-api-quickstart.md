<properties
   pageTitle="Γρήγορη έναρξη για το γράφημα Azure AD API | Microsoft Aure"
   description="Το API Azure Active Directory Graph παρέχει πρόσβαση μέσω προγραμματισμού Azure AD μέσω OData REST API τελικά σημεία. Εφαρμογές μπορούν να χρησιμοποιήσουν το API του γραφήματος για να εκτελέσετε δημιουργία, ανάγνωση, ενημέρωση και διαγραφή λειτουργίες (CRUD) στον κατάλογο δεδομένων και τα αντικείμενα."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Γρήγορη έναρξη για το γράφημα Azure AD API

Το API Graph Azure Active Directory (AD) παρέχει πρόσβαση μέσω προγραμματισμού για να Azure AD μέσω OData REST API τελικά σημεία. Εφαρμογές μπορούν να χρησιμοποιήσουν το API του γραφήματος για να εκτελέσετε δημιουργία, ανάγνωση, ενημέρωση και διαγραφή λειτουργίες (CRUD) στον κατάλογο δεδομένων και τα αντικείμενα. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε το API του γραφήματος για να δημιουργήσετε έναν νέο χρήστη, προβολή ή ενημέρωση των ιδιοτήτων του χρήστη, αλλαγή κωδικού πρόσβασης ενός χρήστη, έλεγχος πρόσβασης βάσει ρόλων συμμετοχής σε ομάδα, απενεργοποίηση ή διαγραφή χρήστη. Για περισσότερες πληροφορίες σχετικά με τις δυνατότητες Graph API και σεναρίων εφαρμογών, ανατρέξτε στο θέμα [API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) και [τις προϋποθέσεις API Azure AD Graph](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure AD Graph API λειτουργίες είναι επίσης διαθέσιμη μέσω του [Microsoft Graph](https://graph.microsoft.io/), ένα ενοποιημένες API που περιλαμβάνει API από άλλες υπηρεσίες της Microsoft, όπως το Outlook, το OneDrive, το OneNote, οργάνωση και Office Graph, προσβάσιμη μέσω ένα μεμονωμένο τελικό σημείο και με διακριτικό πρόσβασης μόνο.

## <a name="how-to-construct-a-graph-api-url"></a>Πώς μπορείτε να δημιουργήσετε μια διεύθυνση URL API Graph

Στο γράφημα API, για να αποκτήσετε πρόσβαση καταλόγου δεδομένων και αντικειμένων (με άλλα λόγια, πόρους ή οντοτήτων) βάσει των οποίων θέλετε να εκτελέσετε λειτουργίες CRUD, μπορείτε να χρησιμοποιήσετε διευθύνσεις URL με βάση το πρωτόκολλο Άνοιγμα δεδομένων (OData). Οι διευθύνσεις URL που χρησιμοποιούνται στο γράφημα API αποτελείται από τα τέσσερα κύρια μέρη: υπηρεσίας ρίζα, αναγνωριστικό μισθωτή, διαδρομή πόρου και επιλογές συμβολοσειρά ερωτήματος: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Λαμβάνει το παράδειγμα την παρακάτω διεύθυνση URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Υπηρεσία ρίζα**: στο Azure AD Graph API, το ριζικό κατάλογο της υπηρεσίας είναι πάντα https://graph.windows.net.
- **Αναγνωριστικό μισθωτή**: Αυτό μπορεί να είναι ένα όνομα επαληθευτεί τομέα (καταχωρημένες), στο παραπάνω παράδειγμα, contoso.com. Μπορεί να είναι ένα Αναγνωριστικό αντικειμένου μισθωτή ή "myorganiztion" ή "me" ψευδώνυμο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσθήκη διεύθυνσης σε οντοτήτων και λειτουργιών στο Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Διαδρομή πόρου**: Αυτή η ενότητα μιας διεύθυνσης URL προσδιορίζει τον πόρο μέχρι να κάποια ενέργεια με (χρήστες, ομάδες, ένα συγκεκριμένο χρήστη, ή μια συγκεκριμένη ομάδα, κ.λπ.) Στο παραπάνω παράδειγμα, είναι ανώτατου επιπέδου "ομάδες" διεύθυνση που ρύθμιση πόρου. Επίσης μπορείτε να αντιμετωπίσετε ένα συγκεκριμένο οντότητα, για παράδειγμα "χρήστες / {objectId}" ή "χρήστες userPrincipalName".
- **Παράμετροι ερωτήματος**:; διαχωρίζει ενότητα διαδρομή πόρων από την ενότητα παράμετροι ερωτήματος. Η παράμετρος ερωτήματος "api έκδοσης" απαιτείται σε όλες τις αιτήσεις στο Graph API. Το API Graph υποστηρίζει επίσης τις ακόλουθες επιλογές ερωτήματος OData: **$filter**, **$orderby**, **αναπτύξτε το στοιχείο $**, **$top**και **$format**. Τις ακόλουθες επιλογές ερωτήματος δεν υποστηρίζονται αυτήν τη στιγμή: **$count**, **$inlinecount**και **$skip**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [υποστηριζόμενες ερωτήματα, φίλτρα, και επιλογές σελιδοποίησης στο API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Εκδόσεις Graph API

Μπορείτε να καθορίσετε την έκδοση για ένα αίτημα Graph API στην παράμετρο ερωτήματος "api έκδοσης". Για την έκδοση 1,5 και νεότερες εκδόσεις, μπορείτε να χρησιμοποιήσετε μια έκδοση αριθμητική τιμή. έκδοση API = 1.6. Για παλαιότερες εκδόσεις, μπορείτε να χρησιμοποιήσετε μια συμβολοσειρά ημερομηνίας που συμμορφώνεται με τη μορφή εεεε-MM-DD- Για παράδειγμα,-έκδοση api = 2013-11-08. Για προεπισκόπηση δυνατότητες, χρησιμοποιήστε τη συμβολοσειρά "βήτα"; Για παράδειγμα, api έκδοσης = beta. Για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ των εκδόσεων API του γραφήματος, ανατρέξτε στο θέμα [Διαχείριση εκδόσεων API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Γράφημα API μετα-δεδομένων

Για να επιστρέψετε στο αρχείο μετα-δεδομένων Graph API, προσθέστε το τμήμα "$metadata" μετά το αναγνωριστικό μισθωτή στο παράδειγμα διεύθυνσης URL για, την παρακάτω διεύθυνση URL επιστρέφει μετα-δεδομένων για την εταιρεία επίδειξης που χρησιμοποιούνται από την Εξερεύνηση Graph: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Μπορείτε να εισαγάγετε αυτήν τη διεύθυνση URL στη γραμμή διευθύνσεων από ένα πρόγραμμα περιήγησης web για να δείτε τα μετα-δεδομένα. Το έγγραφο μετα-δεδομένων CSDL επιστρέφονται περιγράφει το οντοτήτων και σύνθετοι τύποι, τις ιδιότητές τους, και τις συναρτήσεις και ενέργειες που εκτίθεται από την έκδοση του Graph API που ζητήθηκε. Παράλειψη της παραμέτρου έκδοση api θα επιστρέψει μετα-δεδομένα για την πιο πρόσφατη έκδοση.

## <a name="common-queries"></a>Κοινά ερωτήματα

[Κοινά ερωτήματα API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) παραθέτει κοινά ερωτήματα που μπορούν να χρησιμοποιηθούν με Azure AD γραφήματος, όπως τα ερωτήματα που μπορεί να χρησιμοποιηθεί για πρόσβαση σε πόρους ανώτατου επιπέδου στον κατάλογό σας και για να εκτελέσετε λειτουργίες στον κατάλογό σας.

Για παράδειγμα, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` επιστρέφει στοιχεία για contoso.com καταλόγου της εταιρείας.

Ή `https://graph.windows.net/contoso.com/users?api-version=1.6` παραθέτει όλα τα αντικείμενα χρήστη του καταλόγου contoso.com.

## <a name="using-the-graph-explorer"></a>Χρησιμοποιώντας την Εξερεύνηση Graph

Μπορείτε να χρησιμοποιήσετε την Εξερεύνηση Graph για το API Azure AD Graph ερώτημα για τα δεδομένα καταλόγου καθώς δημιουργείτε την εφαρμογή σας.

> [AZURE.IMPORTANT] Εξερεύνηση του Graph δεν υποστηρίζει γράφετε ή να διαγράψετε τα δεδομένα από έναν κατάλογο. Μπορείτε να εκτελέσετε μόνο λειτουργίες ανάγνωσης στον κατάλογό σας Azure AD με την Εξερεύνηση Graph.

Τα παρακάτω είναι το αποτέλεσμα που θα εμφανίζονται εάν επρόκειτο να μεταβείτε στην Εξερεύνηση Graph, επιλέξτε χρήση επίδειξη εταιρεία και εισαγάγετε `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` για να εμφανίσετε όλους τους χρήστες στον κατάλογο επίδειξη:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Φόρτωση της Εξερεύνησης Graph**: για να φορτώσετε το εργαλείο, μεταβείτε στο [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Κάντε κλικ στην επιλογή **Χρήση επίδειξη εταιρείας** για να εκτελεστεί η Εξερεύνηση γράφημα σε δεδομένα από ένα μισθωτή του δείγματος. Δεν χρειάζεται διαπιστευτήρια για να χρησιμοποιήσετε την εταιρεία επίδειξης. Εναλλακτικά, μπορείτε να κάντε κλικ στην επιλογή **Είσοδος** και να συνδεθείτε με τα διαπιστευτήριά σας Azure AD λογαριασμό για να εκτελέσετε την Εξερεύνηση Graph σε σχέση με το μισθωτή. Εάν εκτελείτε Graph Explorer σε σχέση με το δικό σας μισθωτή, εσείς ή ο διαχειριστής θα πρέπει να τη συγκατάθεσή σας κατά την είσοδο. Εάν έχετε μια συνδρομή στο Office 365, έχετε αυτόματα ένα μισθωτή του Azure AD. Τα διαπιστευτήρια που χρησιμοποιείτε για είσοδο Office 365 είναι στην πραγματικότητα, λογαριασμοί Azure AD, και μπορείτε να χρησιμοποιήσετε αυτά τα διαπιστευτήρια με την Εξερεύνηση Graph.

**Εκτέλεση ενός ερωτήματος**: για να εκτελέσετε ένα ερώτημα, πληκτρολογήστε το ερώτημά σας στο πλαίσιο κειμένου αίτηση και κάντε κλικ στην επιλογή **ΛΉΨΗ** ή κάντε κλικ στην επιλογή **Εισαγωγή** αριθμού-κλειδιού. Τα αποτελέσματα εμφανίζονται στο πλαίσιο απάντησης. Για παράδειγμα, `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` θα εμφανιστούν όλα τα αντικείμενα ομάδας στον κατάλογο επίδειξη.

Λάβετε υπόψη τις παρακάτω δυνατότητες και περιορισμοί της Εξερεύνησης του γραφήματος:
- Ορίζει τη δυνατότητα αυτόματης καταχώρησης σε πόρο. Για να δείτε αυτό, κάντε κλικ στην επιλογή **Χρήση επίδειξη εταιρείας** και, στη συνέχεια, κάντε κλικ στο πλαίσιο κειμένου αίτηση (όπου εμφανίζεται η διεύθυνση URL εταιρείας). Μπορείτε να επιλέξετε έναν πόρο ρύθμιση από την αναπτυσσόμενη λίστα.

- Υποστηρίζει το "me" και "myorganization" Προσθήκη διεύθυνσης σε ψευδώνυμα. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε `https://graph.windows.net/me?api-version=1.6` για να επιστρέψετε στο αντικείμενο χρήστη του συνδεδεμένοι στο χρήστη ή `https://graph.windows.net/myorganization/users?api-version=1.6` για να επιστρέψετε όλους τους χρήστες στον τρέχοντα κατάλογο. Σημειώστε ότι χρησιμοποιώντας την "me" ψευδώνυμο επιστρέφει ένα σφάλμα για την εταιρεία επίδειξης επειδή δεν υπάρχει καμία συνδεδεμένοι στο χρήστη που κάνει το αίτημα.

- Μια ενότητα κεφαλίδες απόκρισης. Αυτό μπορεί να χρησιμοποιηθεί για να βοηθήσετε στην αντιμετώπιση προβλημάτων που παρουσιάζονται κατά την εκτέλεση ερωτημάτων.

- Ένα πρόγραμμα προβολής JSON για την απόκριση με ανάπτυξη και σύμπτυξη δυνατότητες.

- Δεν υπάρχει υποστήριξη για την εμφάνιση μικρογραφιών φωτογραφίας.

## <a name="using-fiddler-to-write-to-the-directory"></a>Χρήση Fiddler για να γράψετε στον κατάλογο

Για τους σκοπούς αυτού του Οδηγού γρήγορης έναρξης, μπορείτε να χρησιμοποιήσετε το πρόγραμμα εντοπισμού σφαλμάτων Web Fiddler προκειμένου να εξασκηθείτε απόδοση λειτουργίες 'εγγραφή' σε σχέση με που καταλόγου Azure AD. Για περισσότερες πληροφορίες και να εγκαταστήσετε το Fiddler, ανατρέξτε στο θέμα [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Στο παρακάτω παράδειγμα, θα χρησιμοποιήσετε το πρόγραμμα εντοπισμού σφαλμάτων Web Fiddler για να δημιουργήσετε μια νέα ομάδα ασφαλείας 'MyTestGroup' στον κατάλογό σας Azure AD.

**Αποκτήστε ένα διακριτικό πρόσβασης**: για να αποκτήσετε πρόσβαση Azure AD Graph, απαιτούνται οι υπολογιστές-πελάτες με επιτυχία ο έλεγχος ταυτότητας Azure AD πρώτα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Σενάρια ελέγχου ταυτότητας για Azure AD](active-directory-authentication-scenarios.md).

**Σύνθεση και εκτέλεση ενός ερωτήματος**: ακολουθήστε τα παρακάτω βήματα.

1. Ανοίξτε το πρόγραμμα εντοπισμού σφαλμάτων Web Fiddler και μεταβείτε στην καρτέλα **σύνθεσης** .
2. Επειδή θέλετε να δημιουργήσετε μια νέα ομάδα ασφαλείας, επιλέξτε **καταχώρηση** ως μέθοδο HTTP από το αναπτυσσόμενο μενού. Για περισσότερες πληροφορίες σχετικά με τις λειτουργίες και τα δικαιώματα σε ένα αντικείμενο ομάδας, ανατρέξτε στο θέμα [ομάδα](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) εντός του [Azure AD Graph REST API αναφοράς](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. Στο πεδίο δίπλα στην επιλογή **Δημοσίευση**, πληκτρολογήστε τα εξής, ανάλογα με τη διεύθυνση URL αίτηση: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Θα πρέπει να αντικαταστήσετε το mytenantdomain με το όνομα τομέα του δικού σας καταλόγου Azure AD.

4. Στο πεδίο ακριβώς κάτω από την καταχώρηση ελκυστική προς τα κάτω, πληκτρολογήστε τα εξής:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] SUBSTITUTE σας &lt;το διακριτικό πρόσβασης&gt; με το διακριτικό πρόσβασης για το καταλόγου Azure AD.

5. Στο πεδίο **σώμα αίτησης** , πληκτρολογήστε τα εξής:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ομάδων, ανατρέξτε στο θέμα [Δημιουργία ομάδας](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Για περισσότερες πληροφορίες σχετικά με αντικείμενα Azure AD και τους τύπους που εκτίθενται από Graph και πληροφορίες σχετικά με τις λειτουργίες που μπορούν να εκτελεστούν σε αυτές με το γράφημα, ανατρέξτε στο θέμα [Azure AD Graph REST API αναφορά](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε περισσότερα σχετικά με το [API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Μάθετε περισσότερα σχετικά με το [δικαίωμα API Azure AD γράφημα εύρους](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
