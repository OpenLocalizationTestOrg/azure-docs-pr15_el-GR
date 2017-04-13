<properties 
    pageTitle="Οδηγός γρήγορης εκκίνησης: μηχανικής εκμάθησης συστάσεις API | Microsoft Azure" 
    description="Azure μηχανικής εκμάθησης συστάσεις - Οδηγός γρήγορης εκκίνησης" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Οδηγός γρήγορης εκκίνησης για το API μηχανικής εκμάθησης συστάσεων

>[AZURE.NOTE]Που θα πρέπει να ξεκινήσετε να χρησιμοποιείτε την υπηρεσία γνωστική API συστάσεις αντί για αυτήν την έκδοση. Η υπηρεσία γνωστική συστάσεις θα αντικατάστασης αυτήν την υπηρεσία και όλες τις νέες δυνατότητες θα αναπτυχθούν εκεί. Έχει νέες δυνατότητες όπως δέσμης για υποστήριξη, καλύτερη API Explorer, μια πιο καθαρή εμπειρία επιφάνειας, πιο συνεπείς signup/χρέωση API, κ.λπ.
> Μάθετε περισσότερα σχετικά με [τη μετεγκατάσταση για τη νέα υπηρεσία γνωστική](http://aka.ms/recomigrate)


Αυτό το έγγραφο περιγράφει πώς να ενσωματωμένη την υπηρεσία ή την εφαρμογή για να χρησιμοποιήσετε το Microsoft Azure μηχανικής εκμάθησης συστάσεις. Μπορείτε να βρείτε περισσότερες λεπτομέρειες σε το API συστάσεις στη [συλλογή](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Γενική επισκόπηση

Για να χρησιμοποιήσετε Azure μηχανικής εκμάθησης συστάσεις, πρέπει να ακολουθήσετε τα παρακάτω βήματα:

* Δημιουργήστε ένα μοντέλο - μοντέλου είναι ένα κοντέινερ σας δεδομένων χρήσης, στον κατάλογο δεδομένων και το μοντέλο σύσταση.
* Εισαγωγή στον κατάλογο δεδομένων - κατάλογο περιέχει πληροφορίες μετα-δεδομένα σχετικά με τα στοιχεία. 
* Εισαγωγή δεδομένων χρήσης - δεδομένων χρήσης μπορεί να αποσταλεί σε έναν από τους τρόπους 2 (ή και τα δύο):
    * Με την αποστολή ενός αρχείου που περιέχει τα δεδομένα χρήσης.
    * Με την αποστολή δεδομένων acquisition συμβάντα.
    Συνήθως μπορείτε να αποστείλετε ένα αρχείο χρήση προκειμένου να έχουν τη δυνατότητα να δημιουργήσετε ένα μοντέλο αρχική σύσταση (εκκίνησης) και να χρησιμοποιήσετε μέχρι το σύστημα συγκεντρώνει αρκετά δεδομένα, χρησιμοποιώντας τη μορφή acquisition δεδομένων.
* Δημιουργήστε ένα μοντέλο σύσταση - αυτή είναι μια ασύγχρονη λειτουργία με την οποία το σύστημα σύσταση λαμβάνει όλα τα δεδομένα χρήσης και δημιουργεί ένα μοντέλο σύσταση. Αυτή η λειτουργία μπορεί να διαρκέσει αρκετά λεπτά ή μερικές ώρες ανάλογα με το μέγεθος των δεδομένων και τη ρύθμιση παραμέτρων Δόμηση. Κατά την ενεργοποίηση δημιουργία του, θα λάβετε ένα αναγνωριστικό Δόμηση. Χρησιμοποιήστε το για να ελέγξετε όταν έχει τερματιστεί η διαδικασία δημιουργίας πριν από την εκκίνηση για την εκμετάλλευση συστάσεις.
* Χρήση συστάσεις - λήψη συστάσεις για ένα συγκεκριμένο στοιχείο ή τη λίστα των στοιχείων.

Όλα τα παραπάνω βήματα γίνεται μέσω του API συστάσεις Azure μηχανικής εκμάθησης.  Μπορείτε να κάνετε λήψη ενός δείγματος εφαρμογής που υλοποιεί κάθε ένα από αυτά τα βήματα από το [καθώς και τη συλλογή.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Περιορισμοί

* Ο μέγιστος αριθμός των μοντέλων ανά συνδρομή είναι 10.
* Ο μέγιστος αριθμός των στοιχείων που μπορεί να διατηρήσει έναν κατάλογο είναι 100,000.
* Ο μέγιστος αριθμός των σημείων χρήση που διατηρούνται είναι ~ 5,000,000. Από το παλαιότερο θα διαγραφεί εάν νέα θα αποσταλεί ή αναφερθεί.
* Το μέγιστο μέγεθος των δεδομένων που μπορούν να αποσταλούν σε ΔΗΜΟΣΊΕΥΣΗ (π.χ. εισαγωγή καταλόγου δεδομένων, εισαγωγή δεδομένων χρήσης) είναι 200MB.
* Είναι ο αριθμός των συναλλαγών ανά δευτερόλεπτο για μια έκδοση μοντέλο σύσταση που δεν είναι ενεργή ~ 2TPS. Μια Δόμηση μοντέλο σύσταση που είναι ενεργό να κρατήσετε προς τα επάνω για να 20TPS.

##<a name="integration"></a>Ενοποίηση

###<a name="authentication"></a>Έλεγχος ταυτότητας
Micosoft Azure Marketplace υποστηρίζει το Basic ή το διακριτικό μεθόδου ελέγχου ταυτότητας. Μπορείτε εύκολα να βρείτε τα πλήκτρα λογαριασμού, μεταβαίνοντας τα πλήκτρα στην αγορά στην περιοχή [ρυθμίσεις του λογαριασμού σας](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Βασικός έλεγχος ταυτότητας
Προσθέστε την κεφαλίδα εξουσιοδότησης:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Μετατροπή σε Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Μετατροπή σε Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Υπηρεσία URI
Ο ριζικός κατάλογος υπηρεσίας URI για τα API συστάσεις Azure μηχανικής εκμάθησης είναι [εδώ.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Την πλήρη υπηρεσία URI εκφράζεται χρησιμοποιώντας στοιχεία της προδιαγραφής OData.

###<a name="api-version"></a>Η έκδοση API
Κάθε κλήση API θα έχει, στο τέλος, μιας παραμέτρου ερωτήματος που ονομάζεται apiVersion που πρέπει να οριστεί σε "1.0".

###<a name="ids-are-case-sensitive"></a>Αναγνωριστικά γίνεται διάκριση πεζών-κεφαλαίων
Αναγνωριστικά, επιστρέφεται από οποιοδήποτε από τα API, γίνεται διάκριση πεζών-κεφαλαίων και πρέπει να χρησιμοποιείται ως τέτοια όταν μεταβιβάζονται ως παράμετροι στις επόμενες κλήσεις API. Για παράδειγμα, μοντέλο αναγνωριστικά και τα αναγνωριστικά καταλόγου είναι διάκριση πεζών-κεφαλαίων.

###<a name="create-a-model"></a>Δημιουργήστε ένα μοντέλο
Δημιουργία πρόσκλησης σε "Δημιουργία μοντέλου":

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΔΗΜΟΣΊΕΥΣΗ     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
|   modelName   |   Μόνο γράμματα (A-Ω, α-ω), αριθμούς (0-9), ενωτικά (-) και επιτρέπονται υπογράμμισης (_).<br>Μέγιστος αριθμός χαρακτήρων: 20 |
|   apiVersion      | 1.0 |
|||
| Σώμα αίτησης | ΚΑΝΈΝΑΣ |


**Απάντηση**:

Ο κωδικός κατάστασης HTTP: 200

- `feed/entry/content/properties/id`-Περιέχει το αναγνωριστικό του μοντέλου.
**Σημείωση**: το Αναγνωριστικό μοντέλο είναι διάκριση πεζών-κεφαλαίων.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Εισαγωγή στον κατάλογο δεδομένων

Αν κάνετε αποστολή διάφορα αρχεία στον κατάλογο στο ίδιο μοντέλο με αρκετές κλήσεις, θα σας θα εισαγάγει μόνο τα νέα στοιχεία στον κατάλογο. Υπάρχοντα στοιχεία θα παραμείνουν με τις αρχικές τιμές.

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΔΗΜΟΣΊΕΥΣΗ     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
|   modelId |   Μοναδικό αναγνωριστικό του μοντέλου (με διάκριση πεζών-κεφαλαίων)  |
| όνομα αρχείου | Μορφή κειμένου αναγνωριστικό του καταλόγου.<br>Μόνο γράμματα (A-Ω, α-ω), αριθμούς (0-9), ενωτικά (-) και επιτρέπονται υπογράμμισης (_).<br>Μέγιστο μήκος: 50 |
|   apiVersion      | 1.0 |
|||
| Σώμα αίτησης | Του καταλόγου δεδομένων. Μορφή:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Όνομα</th><th>Υποχρεωτική</th><th>Τύπος</th><th>Περιγραφή</th></tr><tr><td>Αναγνωριστικό στοιχείου</td><td>Ναι</td><td>Αλφαριθμητικά, μέγιστο μήκος 50</td><td>Μοναδικό αναγνωριστικό του στοιχείου</td></tr><tr><td>Όνομα στοιχείου</td><td>Ναι</td><td>Αλφαριθμητικά, μέγιστο μήκος των 255 χαρακτήρων</td><td>Όνομα στοιχείου</td></tr><tr><td>Στοιχείο κατηγορίας</td><td>Ναι</td><td>Αλφαριθμητικά, μέγιστο μήκος των 255 χαρακτήρων</td><td>Κατηγορία στην οποία ανήκει το είδος (π.χ. ψήσιμο βιβλία, τραγικές...)</td></tr><tr><td>Περιγραφή</td><td>Όχι</td><td>Αλφαριθμητικά, μέγιστο μήκος 4000</td><td>Περιγραφή αυτού του στοιχείου</td></tr></table><br>Μέγιστο μέγεθος αρχείου είναι 200MB.<br><br>Παράδειγμα:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, βιβλίο<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, το κανάλι που έχετε ξεχάσει: βιβλίου μια Fiction (βιβλίο Byzantium),<br>Βιβλίο 3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework,<br>552a1940-21e4-4399-82bb-594b46d7ed54, συγκράτησης της άγρια ζώα, βιβλίο</pre> |


**Απάντηση**:

Ο κωδικός κατάστασης HTTP: 200

- `Feed\entry\content\properties\LineCount`-Αποδοχή αριθμός γραμμών.
- `Feed\entry\content\properties\ErrorCount`-Αριθμός γραμμών που εισήχθησαν δεν οφείλεται σε ένα μήνυμα σφάλματος.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Εισαγωγή δεδομένων χρήσης

####<a name="uploading-a-file"></a>Αποστολή ενός αρχείου
Αυτή η ενότητα δείχνει τον τρόπο για την αποστολή δεδομένων χρήσης, χρησιμοποιώντας ένα αρχείο. Μπορείτε να καλέσετε αυτό το API πολλές φορές με χρήση δεδομένων. Για όλες τις κλήσεις θα αποθηκευτούν όλων των δεδομένων χρήσης.

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΔΗΜΟΣΊΕΥΣΗ     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
|   modelId |   Μοναδικό αναγνωριστικό του μοντέλου (με διάκριση πεζών-κεφαλαίων) |
| όνομα αρχείου | Μορφή κειμένου αναγνωριστικό του καταλόγου.<br>Μόνο γράμματα (A-Ω, α-ω), αριθμούς (0-9), ενωτικά (-) και επιτρέπονται υπογράμμισης (_).<br>Μέγιστο μήκος: 50 |
|   apiVersion      | 1.0 |
|||
| Σώμα αίτησης | Χρήση δεδομένων. Μορφή:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Όνομα</th><th>Υποχρεωτική</th><th>Τύπος</th><th>Περιγραφή</th></tr><tr><td>Αναγνωριστικό χρήστη</td><td>Ναι</td><td>Αλφαριθμητικά</td><td>Μοναδικό αναγνωριστικό του χρήστη</td></tr><tr><td>Αναγνωριστικό στοιχείου</td><td>Ναι</td><td>Αλφαριθμητικά, μέγιστο μήκος 50</td><td>Μοναδικό αναγνωριστικό του στοιχείου</td></tr><tr><td>Ώρα</td><td>Όχι</td><td>Ημερομηνία σε μορφή: εεεε/μμ/DDTHH:MM:SS (π.χ. 2013/06/20T10:00:00)</td><td>Ώρα δεδομένων</td></tr><tr><td>Συμβάν</td><td>Όχι, εάν που παρέχεται και, στη συνέχεια, πρέπει επίσης να θέσετε ημερομηνίας</td><td>Ένα από τα εξής:<br>• Κάντε κλικ στην επιλογή<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Αγοράς</td><td></td></tr></table><br>Μέγιστο μέγεθος αρχείου είναι 200MB.<br><br>Παράδειγμα:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Απάντηση**:

Ο κωδικός κατάστασης HTTP: 200

- `Feed\entry\content\properties\LineCount`-Αποδοχή αριθμός γραμμών.
- `Feed\entry\content\properties\ErrorCount`-Αριθμός γραμμών που εισήχθησαν δεν οφείλεται σε ένα μήνυμα σφάλματος.
- `Feed\entry\content\properties\FileId`-Αναγνωριστικό του αρχείου.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Χρήση απόκτηση δεδομένων
Αυτή η ενότητα δείχνει πώς μπορείτε να στείλετε συμβάντα σε πραγματικό χρόνο για να Azure μηχανικής εκμάθησης συστάσεις, συνήθως από την τοποθεσία Web.

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΔΗΜΟΣΊΕΥΣΗ     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Σώμα αίτησης| Καταχώρηση δεδομένων συμβάντων για κάθε συμβάν που θέλετε να στείλετε. Θα πρέπει να στείλετε για την ίδια περίοδο λειτουργίας χρήστη ή σε πρόγραμμα περιήγησης το ίδιο Αναγνωριστικό στο πεδίο αναγνωριστικό περιόδου λειτουργίας. (Ανατρέξτε στο θέμα δείγμα του σώματος συμβάν παρακάτω).|


- Παράδειγμα για το συμβάν 'Κάντε κλικ στην επιλογή':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Παράδειγμα για το συμβάν 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Παράδειγμα για το συμβάν 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Παράδειγμα για το συμβάν 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Παράδειγμα για το συμβάν "Αγορά":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Παράδειγμα στέλνει 2 συμβάντα, 'Κάντε κλικ στην επιλογή' και 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Απάντηση**: κωδικός κατάστασης HTTP: 200

###<a name="build-a-recommendation-model"></a>Δημιουργήστε ένα μοντέλο προτάσεων

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΔΗΜΟΣΊΕΥΣΗ     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
| modelId | Μοναδικό αναγνωριστικό του μοντέλου (με διάκριση πεζών-κεφαλαίων)  |
| userDescription | Μορφή κειμένου αναγνωριστικό του καταλόγου. Λάβετε υπόψη ότι εάν χρησιμοποιείτε κενά διαστήματα πρέπει να κωδικοποιήσετε το με % 20 αντί για αυτό. Ανατρέξτε στο θέμα παραπάνω παράδειγμα.<br>Μέγιστο μήκος: 50 |
| apiVersion | 1.0 |
|||
| Σώμα αίτησης | ΚΑΝΈΝΑΣ |

**Απάντηση**:

Ο κωδικός κατάστασης HTTP: 200

Αυτή είναι μια ασύγχρονης API. Θα λάβετε ένα Αναγνωριστικό Δόμηση ως απάντηση. Για να μάθετε όταν η Δόμηση έχει λήξει, πρέπει να καλέσετε το API "Λήψη δημιουργεί κατάστασης από ένα μοντέλο" και εντοπίστε το Αναγνωριστικό Δόμηση στην απάντηση. Σημειώστε ότι ένα build μπορεί να διαρκέσει από λεπτά σε ώρες ανάλογα με το μέγεθος των δεδομένων.

Δεν είναι δυνατό να εκμετάλλευση συστάσεις έως τη Δόμηση λήξει.

Δόμηση έγκυρη κατάσταση:

- Δημιουργία – που δημιουργήθηκε το μοντέλο.
- Στην ουρά – ενεργοποιήθηκε μοντέλο Δόμηση και βρίσκεται στην ουρά.
- Δημιουργείται δόμησης – μοντέλο.
- Δόμηση επιτυχίας – τερματίστηκε με επιτυχία.
- Σφάλμα – Δημιουργία λήξει με ένα σφάλμα.
- Ακυρώθηκε – δόμηση ακυρώθηκε.
- Ακύρωση – δημιουργία ακυρώνεται.


Σημειώστε ότι μπορείτε να βρείτε το Αναγνωριστικό Δόμηση στην ακόλουθη διαδρομή:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Λήψη κατάσταση δημιουργίας ενός μοντέλου

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΛΉΨΗ     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
|   modelId         |   Μοναδικό αναγνωριστικό του μοντέλου (με διάκριση πεζών-κεφαλαίων)    |
|   onlyLastBuild   |   Υποδεικνύει εάν θέλετε να επιστραφεί όλο το ιστορικό δόμηση του μοντέλου ή μόνο την κατάσταση των την πιο πρόσφατη δομή. |
|   apiVersion      |   1.0                                 |


**Απάντηση**:

Ο κωδικός κατάστασης HTTP: 200

Η απόκριση περιλαμβάνει μια εγγραφή ανά Δόμηση. Κάθε καταχώρηση έχει τα εξής δεδομένα:

- `feed/entry/content/properties/UserName`– Το όνομα του χρήστη.
- `feed/entry/content/properties/ModelName`– Το όνομα του μοντέλου.
- `feed/entry/content/properties/ModelId`– Μοναδικό αναγνωριστικό μοντέλο.
- `feed/entry/content/properties/IsDeployed`– Εάν η Δόμηση έχει αναπτυχθεί (ρομποτική ενεργό build).
- `feed/entry/content/properties/BuildId`– Δημιουργήστε μοναδικό αναγνωριστικό.
- `feed/entry/content/properties/BuildType`-Τύπος Δημιουργία.
- `feed/entry/content/properties/Status`– Δημιουργήστε κατάστασης. Μπορεί να είναι ένα από τα εξής: σφάλμα, δόμησης, σε ουρά, Cancelling, ακυρώθηκε, επιτυχίας
- `feed/entry/content/properties/StatusMessage`– Μήνυμα λεπτομερή κατάσταση (ισχύει μόνο για συγκεκριμένο Πολιτείες).
- `feed/entry/content/properties/Progress`– Δημιουργήστε την πρόοδο (%).
- `feed/entry/content/properties/StartTime`– Δημιουργήστε ώρα έναρξης.
- `feed/entry/content/properties/EndTime`– Δημιουργήστε ώρα λήξης.
- `feed/entry/content/properties/ExecutionTime`– Δημιουργήστε διάρκεια.
- `feed/entry/content/properties/ProgressStep`– Λεπτομέρειες σχετικά με το τρέχον στάδιο στο οποίο μια Δόμηση σε εξέλιξη.

Δόμηση έγκυρη κατάσταση:
- Δημιουργήθηκε – δημιουργία αίτησης καταχώρησης που δημιουργήθηκε.
- Στην ουρά – δημιουργία αίτησης ενεργοποιήθηκε και βρίσκεται στην ουρά.
- Δόμησης – δημιουργία βρίσκεται σε εξέλιξη.
- Δόμηση επιτυχίας – τερματίστηκε με επιτυχία.
- Σφάλμα – Δημιουργία λήξει με ένα σφάλμα.
- Ακυρώθηκε – δόμηση ακυρώθηκε.
- Ακύρωση – δημιουργία ακυρώνεται.

Οι έγκυρες τιμές για δημιουργία τύπο:
- Κατάταξη - δημιουργία κατάταξη. (Για τη συνάρτηση rank Δόμηση λεπτομέρειες, ανατρέξτε στο έγγραφο "Τεκμηρίωση μηχανικής εκμάθησης σύσταση API".)
- Σύσταση - δημιουργία προτάσεων.
- Fbt - συχνά αγοράσατε μαζί Δόμηση.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Λήψη συστάσεων

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΛΉΨΗ     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
| modelId | Μοναδικό αναγνωριστικό του μοντέλου (με διάκριση πεζών-κεφαλαίων) |
| ότι τα αναγνωριστικά στοιχείων | Διαχωρισμένες με κόμματα λίστα να συνιστάται για τα στοιχεία.<br>Μέγιστο μήκος: 1024 |
| numberOfResults | Αριθμός των αποτελεσμάτων που απαιτείται |
| includeMetatadata | Μελλοντική χρήση, πάντα false |
| apiVersion | 1.0 |

**Απόκριση:**

Ο κωδικός κατάστασης HTTP: 200

Η απόκριση περιλαμβάνει μια εγγραφή ανά στοιχείο συνιστάται. Κάθε καταχώρηση έχει τα εξής δεδομένα:

- `Feed\entry\content\properties\Id`-Αναγνωριστικό στοιχείου συνιστάται.
- `Feed\entry\content\properties\Name`-Το όνομα του στοιχείου.
- `Feed\entry\content\properties\Rating`-Χαρακτηρισμός η σύσταση; υψηλότερη εμπιστοσύνης σημαίνει ότι το μεγαλύτερο αριθμό.
- `Feed\entry\content\properties\Reasoning`-Σύσταση λογικής (π.χ. σύσταση εξηγήσεις).

OData XML

Το παρακάτω παράδειγμα απάντηση περιλαμβάνει 10 προτεινόμενα στοιχείων:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Ενημέρωση μοντέλου
Μπορείτε να ενημερώσετε την περιγραφή μοντέλο ή το αναγνωριστικό του ενεργού Δόμηση.
*Ενεργό Δόμηση ID* - κάθε Δόμηση για κάθε μοντέλο περιλαμβάνει ένα αναγνωριστικό Δόμηση. Το Αναγνωριστικό ενεργό Δόμηση είναι επιτυχής δημιουργία του πρώτου από κάθε νέο μοντέλο. Όταν έχετε ένα Αναγνωριστικό ενεργό Δόμηση και μπορείτε να κάνετε πρόσθετες δημιουργεί για το ίδιο μοντέλο, πρέπει να ρητά ορίστε την ως το προεπιλεγμένο Αναγνωριστικό Δόμηση, εάν θέλετε να. Κατά την κατανάλωση συστάσεις, εάν δεν καθορίσετε το Αναγνωριστικό Δόμηση που θέλετε να χρησιμοποιήσετε, ο προεπιλεγμένος θα χρησιμοποιηθεί αυτόματα.

Αυτός ο μηχανισμός σας δίνει τη δυνατότητα - μόλις μοντέλου σύσταση στη παραγωγή - για να δημιουργήσετε νέα μοντέλα και να ελέγξετε τους πριν από την προώθηση τους παραγωγή.

| Μέθοδος HTTP | URI |
|:--------|:--------|
|ΤΟΠΟΘΈΤΗΣΗ     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Παράδειγμα:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Όνομα παραμέτρου  |   Έγκυρες τιμές                        |
|:--------          |:--------                              |
| αναγνωριστικό | Μοναδικό αναγνωριστικό του μοντέλου (με διάκριση πεζών-κεφαλαίων) |
| apiVersion | 1.0 |
|||
| Σώμα αίτησης | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Σημειώστε ότι οι ετικέτες XML, περιγραφή και ActiveBuildId είναι προαιρετικό. Εάν δεν θέλετε να ορίσετε περιγραφή ή ActiveBuildId, καταργήστε την ετικέτα ολόκληρο. |

**Απάντηση**:

Ο κωδικός κατάστασης HTTP: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Νομικά
Σε αυτό το έγγραφο παρέχεται "ως-είναι". Πληροφορίες και τις προβολές εκφρασμένη σε αυτό το έγγραφο, συμπεριλαμβανομένων των URL και άλλων αναφορών σε τοποθεσία Web στο Internet, ενδέχεται να αλλάξουν χωρίς ειδοποίηση. Ορισμένα παραδείγματα παρουσιάζεται στο παρόν παρέχονται μόνο για επεξήγηση και είναι φανταστικά. Χωρίς σύνδεση ή πραγματικό συσχετισμού προορίζεται ή να αναφέρονται. Αυτό το έγγραφο δεν σας παρέχει δικαιώματα σε οποιαδήποτε πνευματικής ιδιοκτησίας σε κάθε προϊόν της Microsoft. Μπορείτε να αντιγράψετε και να χρησιμοποιήσετε αυτό το έγγραφο για εσωτερικούς σκοπούς αναφοράς. © 2014 Microsoft. Πνευματικά δικαιώματα. 
 
