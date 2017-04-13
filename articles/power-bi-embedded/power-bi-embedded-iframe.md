<properties
   pageTitle="Πώς να χρησιμοποιείτε το Power BI ενσωματωμένο με το ΥΠΌΛΟΙΠΟ | Microsoft Azure"
   description="Μάθετε πώς να χρησιμοποιείτε το Power BI ενσωματωμένο με το ΥΠΌΛΟΙΠΟ "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Πώς να χρησιμοποιείτε το Power BI ενσωματωμένο με το ΥΠΌΛΟΙΠΟ


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI ενσωματωμένων: Τι είναι και τι είναι για
Μια επισκόπηση του Power BI ενσωματωμένου περιγράφεται στην Επίσημη [τοποθεσία Power BI ενσωματωμένο](https://azure.microsoft.com/services/power-bi-embedded/), αλλά ας ρίξουμε μια γρήγορη ματιά πριν λαμβάνουμε τις λεπτομέρειες σχετικά με τη χρήση του με το ΥΠΌΛΟΙΠΟ.

Είναι πραγματικά πολύ απλές. Μια ISV θέλει συχνά να χρησιμοποιήσετε τις απεικονίσεις δυναμικά δεδομένα του [Power BI](https://powerbi.microsoft.com) στην εφαρμογή δικό τους ως μπλοκ δόμησης περιβάλλοντος εργασίας Χρήστη.

Ωστόσο, γνωρίζετε, η ενσωμάτωση αναφορές του Power BI ή τα πλακίδια στην ιστοσελίδα σας είναι ήδη πιθανές χωρίς την υπηρεσία Power BI ενσωματωμένου Azure, με τη χρήση του **API του Power BI**. Όταν θέλετε να κάνετε κοινή χρήση αναφορών σας στην ίδια εταιρεία σας, μπορείτε να ενσωματώσετε τις αναφορές με έλεγχο ταυτότητας Azure AD. Ο χρήστης που βλέπει τις αναφορές, πρέπει να συνδεθείτε χρησιμοποιώντας το δικό τους λογαριασμό Azure AD. Όταν θέλετε να μοιραστείτε τις αναφορές για όλους τους χρήστες (συμπεριλαμβανομένων των εξωτερικών χρηστών), μπορείτε να ενσωματώσετε απλώς με ανώνυμη πρόσβαση.

Αλλά μπορείτε να δείτε, αυτό απλή ενσωμάτωση λύσης δεν ικανοποιεί τις ανάγκες μιας εφαρμογής ISV.
Οι περισσότερες εφαρμογές ISV πρέπει να κάνουν τα δεδομένα για τα δικά τους πελάτες, όχι απαραίτητα χρήστες στον οργανισμό δικό τους. Για παράδειγμα, εάν διάρκεια κάποια υπηρεσία για την εταιρεία A και B εταιρείας, οι χρήστες στην εταιρεία A θα πρέπει να δείτε μόνο δεδομένων για την εταιρεία τις δικές τους απαντήσεις. Αυτό σημαίνει ότι η πολλαπλή μίσθωση είναι απαραίτητο για την παράδοση.

Η εφαρμογή ISV επίσης μπορεί να προσφέρει το δικό της μεθόδους ελέγχου ταυτότητας όπως auth φόρμες, βασικές auth, κ.λπ... Στη συνέχεια, η ενσωμάτωση λύση πρέπει να συνεργαστείτε με αυτό υπάρχουσες μεθόδους ελέγχου ταυτότητας με ασφάλεια. Είναι επίσης απαραίτητο για τους χρήστες να μπορούν να χρησιμοποιούν αυτές τις εφαρμογές ISV χωρίς την αγορά επιπλέον ή άδειας χρήσης μιας συνδρομής του Power BI.

 **Power BI ενσωματωμένου** έχει σχεδιαστεί για επακριβώς αυτά τα είδη των ISV σενάρια. Επομένως, τώρα που έχουμε εκτός της περιοχής που γρήγορη εισαγωγή, ας ξεκινήσουμε σε ορισμένες λεπτομέρειες

Μπορείτε να χρησιμοποιήσετε το .NET \(C#) ή Node.js SDK, για να δημιουργήσετε εύκολα την εφαρμογή σας με το Power BI ενσωματωμένο. Αλλά, σε αυτό το άρθρο θα σας θα εξηγούν σχετικά με τη ροή HTTP \(με AuthN) του Power BI χωρίς SDK. Κατανόηση αυτήν τη ροή, μπορείτε να δημιουργήσετε την εφαρμογή **με οποιαδήποτε γλώσσα προγραμματισμού**, και μπορείτε να κατανοήσετε βάθος βασικά στοιχεία του Power BI ενσωματωμένο.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Συλλογή χώρου εργασίας Δημιουργία Power BI και πλήκτρο πρόσβασης get \(Provisioning)
Power BI ενσωματωμένου είναι μία από τις υπηρεσίες του Azure. Μόνο οι ISV που χρησιμοποιεί πύλη Azure χρεώνεται για τέλη χρήσης \(ανά ωριαία περίοδο λειτουργίας χρήστη), και ο χρήστης που βλέπει την αναφορά δεν χρεώνεται ή ακόμα και απαιτεί συνδρομή Azure.
Πριν να ξεκινήσετε την ανάπτυξη εφαρμογών, θα σας πρέπει να δημιουργήσετε τη **συλλογή χώρου εργασίας Power BI** με χρήση του Azure πύλη.

Κάθε χώρο εργασίας του Power BI ενσωματωμένου είναι ο χώρος εργασίας για κάθε πελάτη (μισθωτής) και να προσθέσουμε πολλούς χώρους εργασίας σε κάθε συλλογή χώρου εργασίας. Χρησιμοποιείται το ίδιο πλήκτρο πρόσβασης σε κάθε συλλογή χώρου εργασίας. Εφέ, η συλλογή χώρου εργασίας είναι το όριο ασφαλείας για το Power BI ενσωματωμένο.

![](media\power-bi-embedded-iframe\create-workspace.png)

Όταν θα σας ολοκληρώσετε τη δημιουργία της συλλογής χώρου εργασίας, αντιγράψτε το πλήκτρο πρόσβασης από το Azure πύλη.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Επίσης να προμηθεύσουν τη συλλογή χώρου εργασίας και να λάβετε πλήκτρο πρόσβασης μέσω REST API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [APIs υπηρεσία παροχής του Power BI πόρων](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Δημιουργία αρχείου .pbix με το Power BI Desktop
Στη συνέχεια, θα σας πρέπει να δημιουργήσετε τη σύνδεση δεδομένων και αναφορές για να ενσωματωθεί.
Για αυτήν την εργασία, δεν υπάρχει προγραμματισμού ή κώδικα. Χρησιμοποιούμε απλώς Power BI Desktop.
Σε αυτό το άρθρο θα σας δεν θα δούμε τις λεπτομέρειες σχετικά με τον τρόπο χρήσης του Power BI Desktop. Εάν χρειαστείτε βοήθεια εδώ, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Για παράδειγμά μας, θα απλώς χρησιμοποιήσουμε το [Δείγμα Retail ανάλυσης](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Δημιουργία χώρου εργασίας Power BI

Τώρα που το προμήθεια είναι έτοιμοι, ας ξεκινήσουμε δημιουργώντας έναν πελάτη χώρου εργασίας στη συλλογή χώρου εργασίας μέσω APIs ΥΠΌΛΟΙΠΟ. Το παρακάτω HTTP ΔΗΜΟΣΊΕΥΣΗ αίτηση (REST) δημιουργεί το νέο χώρο εργασίας στη συλλογή μας υπάρχοντα χώρο εργασίας. Στο παράδειγμά μας, το όνομα της συλλογής χώρου εργασίας είναι **mypbiapp**.
Μόλις μπορούμε να εγκαταστήσουμε το πλήκτρο πρόσβασης, το οποίο θα σας αντιγράψατε προηγουμένως, ως **AppKey**. Είναι πολύ απλής ελέγχου ταυτότητας!

**Αίτηση HTTP**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**Απόκριση HTTP**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Το επιστρεφόμενο **workspaceId** χρησιμοποιείται για τις παρακάτω οι επόμενες κλήσεις API. Εφαρμογή μας πρέπει να διατηρήσετε αυτήν την τιμή.

## <a name="import-pbix-file-into-the-workspace"></a>Εισαγωγή αρχείου .pbix στο χώρο εργασίας
Κάθε χώρο εργασίας να φιλοξενήσετε ένα μόνο αρχείο Power BI Desktop με ένα σύνολο δεδομένων \(καθώς και ρυθμίσεις προέλευσης δεδομένων) και αναφορές. Θα σας να εισαγάγετε το αρχείο .pbix στο χώρο εργασίας, όπως φαίνεται στον παρακάτω κώδικα. Όπως μπορείτε να δείτε, θα σας να αποστείλετε τη δυαδική μορφή αρχείου .pbix χρήση πολλαπλών τμημάτων MIME στο http.

Το τμήμα uri **32960a09-6366-4208-a8bb-9e0678cdbb9d** είναι η workspaceId και παραμέτρου ερωτήματος **datasetDisplayName** είναι το όνομα του συνόλου δεδομένων για να δημιουργήσετε. Το σύνολο δεδομένων που έχουν δημιουργηθεί διατηρεί όλα τα δεδομένα που σχετίζονται με εργαλεία σε .pbix αρχείων όπως ως δεδομένων που έχουν εισαχθεί, το δείκτη του ποντικιού στην προέλευση δεδομένων, κ.λπ...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Αυτή η εργασία εισαγωγής μπορεί να εμφανιστούν για κάποιο χρονικό διάστημα. Όταν ολοκληρωθεί η εγκατάσταση, μας εφαρμογής να ζητήσετε από την κατάσταση εργασίας με χρήση του αναγνωριστικού εισαγωγής. Στο παράδειγμά μας, το αναγνωριστικό εισαγωγής είναι **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Τα παρακάτω είναι ζητά κατάσταση χρησιμοποιώντας αυτό το αναγνωριστικό εισαγωγής:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Εάν η εργασία δεν είναι ολοκληρωμένη, η απόκριση HTTP μπορεί να είναι ως εξής:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Εάν η εργασία έχει ολοκληρωθεί, η απόκριση HTTP μπορεί να περισσότερες ως εξής:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Σύνδεση αρχείου προέλευσης δεδομένων \(και πολλαπλή μίσθωση των δεδομένων)
Ενώ σχεδόν όλα τα αντικείμενα στο αρχείο .pbix έχουν εισαχθεί στο μας χώρου εργασίας, τα διαπιστευτήρια για προελεύσεις δεδομένων δεν είναι. Ως αποτέλεσμα, όταν χρησιμοποιείτε τη **λειτουργία DirectQuery**, η αναφορά ενσωματωμένου δεν μπορούν να εμφανιστούν σωστά. Ωστόσο, όταν χρησιμοποιείτε τη **λειτουργία εισαγωγής**, θα σας να προβάλετε την αναφορά χρησιμοποιώντας τα υπάρχοντα δεδομένα που έχουν εισαχθεί. Σε αυτήν την περίπτωση, πρέπει να μπορούμε να εγκαταστήσουμε τα διαπιστευτήρια με τα παρακάτω βήματα μέσω ΥΠΌΛΟΙΠΑ κλήσεις.

Πρώτα, θα πρέπει να αποκτήσω το αρχείο προέλευσης δεδομένων πύλης. Γνωρίζουμε το σύνολο δεδομένων **αναγνωριστικό** είναι το αναγνωριστικό που επιστρέφεται προηγουμένως.

**Αίτηση HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**Απόκριση HTTP**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Χρησιμοποιώντας το επιστρεφόμενο πύλης αναγνωριστικό και αναγνωριστικό datasource \(δείτε την προηγούμενη **gatewayId** και το **αναγνωριστικό** στο αποτέλεσμα), μπορεί να αλλάξουμε τα διαπιστευτήρια της αυτή η προέλευση δεδομένων ως εξής:

**Αίτηση HTTP**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**Απόκριση HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

Παραγωγή, θα σας, επίσης, να ορίσετε τη συμβολοσειρά σύνδεσης διαφορετικό για κάθε χώρο εργασίας χρησιμοποιώντας REST API. \(δηλαδή, θα σας να διαχωρίσετε τη βάση δεδομένων για κάθε πελάτες.)

Το παρακάτω αλλάζει τη συμβολοσειρά σύνδεσης της προέλευσης δεδομένων μέσω ΥΠΌΛΟΙΠΟ.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Εναλλακτικά, μπορούμε να χρησιμοποιήσουμε ασφάλειας σε επίπεδο γραμμών στο Power BI ενσωματωμένο και θα σας να διαχωρίσετε τα δεδομένα για κάθε χρήστες σε μία αναφορά. As a Result, θα σας μπορούν να προμηθεύσουν κάθε αναφορά πελάτη με το ίδιο .pbix \(περιβάλλοντος εργασίας Χρήστη, κ.λπ.) και διαφορετικές προελεύσεις δεδομένων.

> [AZURE.NOTE] Εάν χρησιμοποιείτε τη **λειτουργία εισαγωγής** αντί **στη λειτουργία DirectQuery**, δεν υπάρχει τρόπος να ανανεώσετε μοντέλα μέσω API. Και, στην εσωτερική εγκατάσταση αρχεία προέλευσης δεδομένων μέσω του Power BI πύλης ακόμη δεν υποστηρίζεται στο Power BI ενσωματωμένο. Ωστόσο, που θα θέλετε πραγματικά να να παρακολουθείτε το [ιστολόγιο του Power BI](https://powerbi.microsoft.com/blog/) για το τι νέο υπάρχει και τι έρχεται σε μελλοντικές εκδόσεις.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Έλεγχος ταυτότητας και φιλοξενίας (ενσωμάτωση) εκθέσεις στην ιστοσελίδα μας

Στο προηγούμενο REST API, θα σας να χρησιμοποιήσετε το πλήκτρο πρόσβασης **AppKey** ίδια ως κεφαλίδα εξουσιοδότησης. Επειδή αυτές τις κλήσεις αντιμετώπισης στην πλευρά διακομιστή παρασκηνίου, είναι ασφαλής.

Αλλά, όταν θα σας να ενσωματώσετε την αναφορά σε ιστοσελίδα μας, αυτό το είδος των πληροφοριών ασφαλείας θα αντιμετώπισης χρησιμοποιώντας JavaScript \(frontend). Στη συνέχεια, την τιμή της κεφαλίδας εξουσιοδότησης πρέπει να είναι ασφαλή. Εάν το πλήκτρο πρόσβασης είναι εντοπιστεί από ένα κακόβουλο χρήστη ή κακόβουλο κώδικα, μπορούν να καλέσουν τις λειτουργίες χρησιμοποιώντας αυτό το κλειδί.

Όταν θα σας να ενσωματώσετε την αναφορά σε ιστοσελίδα μας, πρέπει να χρησιμοποιήσουμε το διακριτικό υπολογισμένη αντί για το πλήκτρο πρόσβασης **AppKey**. Πρέπει να δημιουργήσετε το διακριτικό της Web του OAuth Json μας εφαρμογής \(JWT) που αποτελείται από τα αξιώσεων και την υπολογισμένη ψηφιακή υπογραφή. Όπως φαίνεται παρακάτω, αυτό JWT OAuth είναι τα διακριτικά οριοθετημένων κωδικοποιημένη συμβολοσειρά.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Πρώτα, θα σας πρέπει να προετοιμάσετε το εισαγωγής τιμή, η οποία έχει συνδεθεί αργότερα. Αυτή η τιμή είναι συμβολοσειρά url κωδικοποιημένη (rfc4648) base64 το παρακάτω json και αυτές είναι οριοθετημένο από την τελεία \(.) χαρακτήρας. Αργότερα, θα σας θα εξηγούν τον τρόπο για να λάβετε το αναγνωριστικό αναφοράς.

> [AZURE.NOTE] Εάν θέλουμε να χρησιμοποιήσετε ασφάλεια επιπέδου γραμμής (RLS) με το Power BI ενσωματωμένο, θα σας πρέπει επίσης να καθορίσετε **όνομα χρήστη** και **τους ρόλους** στο το αξιώσεων.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Στη συνέχεια, θα σας πρέπει να δημιουργήσετε τη συμβολοσειρά κωδικοποίηση base64 HMAC \(την υπογραφή) με αλγόριθμο SHA256. Αυτή η τιμή συνδεδεμένος εισόδου είναι της προηγούμενης συμβολοσειράς.

Τελευταία, θα σας πρέπει να συνδυάσετε τιμή και υπογραφή συμβολοσειρά εισόδου χρησιμοποιώντας περίοδο \(.) χαρακτήρας. Η συμβολοσειρά ολοκληρωμένη είναι το διακριτικό εφαρμογή για την ενσωμάτωση αναφοράς. Ακόμα και αν το διακριτικό εφαρμογή είναι εντοπιστεί από έναν κακόβουλο χρήστη, δεν είναι δυνατό να λαμβάνουν το αρχικό πλήκτρο πρόσβασης. Αυτό το διακριτικό εφαρμογή θα λήξει γρήγορα.

Ακολουθεί ένα παράδειγμα PHP για αυτά τα βήματα:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Τέλος, ενσωμάτωση την αναφορά στην ιστοσελίδα

Για την ενσωμάτωση μας αναφοράς, θα σας πρέπει να λάβετε τη διεύθυνση url ενσωμάτωση και **αναγνωριστικό** χρησιμοποιώντας την παρακάτω REST API αναφοράς.

**Αίτηση HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**Απόκριση HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Θα σας να ενσωματώσετε την αναφορά στο μας εφαρμογής web με χρήση του προηγούμενου διακριτικού εφαρμογής.
Αν κοιτάξουμε στον επόμενο δείγμα κώδικα, το τμήμα πρώην είναι το ίδιο με το προηγούμενο παράδειγμα. Στο τελευταίο τμήμα, αυτό το δείγμα δείχνει το **embedUrl** \(δείτε το προηγούμενο αποτέλεσμα) στο iframe, και καταχώρηση το διακριτικό εφαρμογής σε iframe.

> [AZURE.NOTE] Θα χρειαστεί να αλλάξετε την τιμή αναγνωριστικό αναφοράς σε μία από τις δικές σας. Επίσης, εξαιτίας ενός σφάλματος στο σύστημά μας Διαχείριση περιεχομένου, την ετικέτα iframe στο δείγμα κώδικα διαβάζεται ως έχουν. Κατάργηση capped κειμένου από την ετικέτα, εάν κάνετε αντιγραφή και επικόλληση σε αυτό το δείγμα κώδικα.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Και το αποτέλεσμα:

![](media\power-bi-embedded-iframe\view-report.png)

Προς το παρόν, ενσωματωμένα Power BI εμφανίζει μόνο την αναφορά στο iframe. Ωστόσο, να παρακολουθείτε το [Ιστολόγιο του Power BI](). Βελτιώσεις για μελλοντική μπορούν να χρησιμοποιήσουν το νέο πελάτη API που θα Επιτρέψτε μας αποστολή πληροφοριών στο iframe, καθώς και λήψη πληροφοριών. Συναρπαστική στοιχείων!


## <a name="see-also"></a>Δείτε επίσης
- [Πραγματοποίηση ελέγχου ταυτότητας και εξουσιοδότησης στο Power BI ενσωματωμένο](power-bi-embedded-app-token-flow.md)
