<properties
    pageTitle="Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Cloud Services | Microsoft Azure"
    description="Μάθετε πώς να εκθέσετε Azure εφαρμογή σας ή των δεδομένων σε προσαρμοσμένο τομέα με ρύθμιση παραμέτρων DNS."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για μια υπηρεσία Azure cloud

> [AZURE.SELECTOR]
- [Πύλη του Azure](cloud-services-custom-domain-name-portal.md)
- [Azure κλασική πύλη](cloud-services-custom-domain-name.md)


Όταν δημιουργείτε μια υπηρεσία Cloud, Azure εκχωρεί σε έναν υποτομέα της cloudapp.net. Για παράδειγμα, εάν την υπηρεσία Cloud ονομάζεται "contoso", οι χρήστες θα μπορούν να έχουν πρόσβαση σε μια διεύθυνση URL όπως http://contoso.cloudapp.net την εφαρμογή σας. Azure αντιστοιχίζει επίσης μια εικονική διεύθυνση IP.

Ωστόσο, μπορείτε επίσης να εκθέσει την εφαρμογή σας στο δικό σας όνομα τομέα, όπως contoso.com. Σε αυτό το άρθρο εξηγεί τον τρόπο να δεσμεύσετε ή να ρυθμίσετε ένα προσαρμοσμένο όνομα τομέα για μια υπηρεσία Cloud web ρόλους.

Να κάνετε που ήδη undestand ποιες εγγραφές CNAME και A; [Μεταπήδηση πέρα από την εξήγηση](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Αποκτήστε γρηγορότερα μεταβαίνοντας! Χρησιμοποιήστε το Azure [αναλυτικές οδηγίες](http://support.microsoft.com/kb/2990804). Βγάζει Συσχετίζοντας ένα προσαρμοσμένο όνομα τομέα και ασφάλεια επικοινωνιών (SSL) με τις υπηρεσίες Cloud Azure ή τοποθεσίες Web Azure γρήγορα.

<p/>

> [AZURE.NOTE]
> Εφαρμόστε τις διαδικασίες σε αυτήν την εργασία με τις υπηρεσίες Cloud Azure. Για τις υπηρεσίες εφαρμογών, ανατρέξτε στο θέμα [αυτό](../app-service-web/web-sites-custom-domain-name.md). Για λογαριασμούς του χώρου αποθήκευσης, ανατρέξτε στο θέμα [αυτό](../storage/storage-custom-domain-name.md).


## <a name="understand-cname-and-a-records"></a>Κατανόηση των εγγραφών CNAME και A

Εγγραφή CNAME (ή ψευδώνυμο records) και εγγραφές και οι δύο σάς επιτρέπουν να συσχετίσετε ένα όνομα τομέα με ένα συγκεκριμένο διακομιστή (ή σε αυτήν την περίπτωση, υπηρεσίας) ωστόσο λειτουργούν διαφορετικά. Υπάρχουν επίσης ορισμένες συγκεκριμένα ζητήματα κατά τη χρήση μιας εγγραφών με τις υπηρεσίες Azure Cloud που πρέπει να εξετάσετε πριν να αποφασίσετε ποιες μπορείτε να χρησιμοποιήσετε.

### <a name="cname-or-alias-record"></a>Εγγραφή CNAME ή ψευδώνυμο

Μια εγγραφή CNAME αντιστοιχίζει ένα *συγκεκριμένο* τομέα, όπως **contoso.com** ή **www.contoso.com**, σε ένα όνομα τομέα κανονικής. Σε αυτήν την περίπτωση, το όνομα τομέα κανονικής είναι το **[myapp] .cloudapp .net** όνομα του τομέα σας Azure φιλοξενούνται εφαρμογής. Μετά τη δημιουργία, την εγγραφή CNAME δημιουργεί ένα ψευδώνυμο για το **[myapp] .cloudapp .net**. Η εγγραφή CNAME θα επίλυση στη διεύθυνση IP του σας **[myapp] .cloudapp .net** υπηρεσίας αυτόματα, ώστε να εάν αλλάξει τη διεύθυνση IP του την υπηρεσία cloud, δεν χρειάζεται να κάνετε κάποια ενέργεια.

> [AZURE.NOTE]
> Ορισμένες μητρώα καταχώρησης ονομάτων τομέα μόνο σάς επιτρέπουν να αντιστοιχίσετε δευτερεύοντες τομείς, όταν χρησιμοποιείτε μια εγγραφή CNAME, όπως www.contoso.com και δεν ριζικό κατάλογο ονομάτων, όπως contoso.com. Για περισσότερες πληροφορίες σχετικά με τις εγγραφές CNAME, ανατρέξτε στην τεκμηρίωση που παρέχεται από το μητρώο καταχώρησης ονομάτων, [την καταχώρηση Wikipedia στην εγγραφή CNAME](http://en.wikipedia.org/wiki/CNAME_record)ή το έγγραφο [IETF ονόματα τομέων – εφαρμογή και προδιαγραφές](http://tools.ietf.org/html/rfc1035) .

### <a name="a-record"></a>Μια εγγραφή

Μια εγγραφή A αντιστοιχίζει ένα τομέα, όπως **contoso.com** ή **www.contoso.com**, *ή έναν τομέα μπαλαντέρ* όπως ** \*. contoso.com**, σε μια διεύθυνση IP. Στην περίπτωση μιας Azure υπηρεσία Cloud, το εικονικό IP της υπηρεσίας. Επομένως το βασικό πλεονέκτημα της εγγραφής A επάνω σε μια εγγραφή CNAME είναι ότι μπορείτε να έχετε μία εγγραφή που χρησιμοποιεί ένα χαρακτήρα μπαλαντέρ, όπως \* **. contoso.com**, που θα χειρισμού των αιτήσεων για πολλούς δευτερεύοντες τομείς όπως **mail.contoso.com**, **login.contoso.com**ή **www.contso.com**.

> [AZURE.NOTE]
> Εφόσον μια εγγραφή A έχει αντιστοιχιστεί σε μια στατική διεύθυνση IP, δεν μπορεί να επιλύσει αυτόματα τις αλλαγές στη διεύθυνση IP του την υπηρεσία Cloud. Η διεύθυνση IP που χρησιμοποιούνται από την υπηρεσία Cloud που έχει εκχωρηθεί η πρώτη φορά που αναπτύσσετε σε μια κενή υποδοχή (παραγωγής ή ενδιάμεσου σταδίου.) Εάν διαγράψετε για την υποδοχή την ανάπτυξη, τη διεύθυνση IP έχει κυκλοφορήσει από Azure και τυχόν μελλοντικές αναπτύξεις στο διάστημα μπορεί να σας δοθεί μια νέα διεύθυνση IP.
>
> Εύκολη, τη διεύθυνση IP μιας δεδομένης ανάπτυξης υποδοχής (παραγωγής ή ενδιάμεσου σταδίου) είναι σταθερές κατά την εναλλαγή μεταξύ ενδιάμεσου σταδίου και αναπτύξεις παραγωγής ή αναβάθμιση στην ίδια θέση μιας υπάρχουσας ανάπτυξης. Για περισσότερες πληροφορίες σχετικά με την εκτέλεση αυτών των ενεργειών, ανατρέξτε στο θέμα [πώς μπορείτε να διαχειριστείτε τις υπηρεσίες cloud](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Προσθέστε μια εγγραφή CNAME για τον προσαρμοσμένο τομέα σας

Για να δημιουργήσετε μια εγγραφή CNAME, που πρέπει να προσθέσετε μια νέα εγγραφή στον πίνακα DNS για τον προσαρμοσμένο τομέα σας, χρησιμοποιώντας τα εργαλεία που παρέχονται από το μητρώο καταχώρησης ονομάτων τομέων. Κάθε μητρώο καταχώρησης ονομάτων τομέων έχει μια παρόμοια αλλά ελαφρώς διαφορετικές μέθοδο που καθορίζει μια εγγραφή CNAME, αλλά οι έννοιες είναι η ίδια.

1. Χρησιμοποιήστε μία από τις παρακάτω μεθόδους για να βρείτε το **. cloudapp.net** το όνομα τομέα που έχουν εκχωρηθεί σε υπηρεσία cloud.

    * Συνδεθείτε στην [πύλη Azure κλασική], επιλέξτε την υπηρεσία cloud, επιλέξτε **τον πίνακα εργαλείων**και, στη συνέχεια, βρείτε την καταχώρηση **Διεύθυνση URL της τοποθεσίας** στην ενότητα **γρήγορη ματιά** .
    
        ![ενότητα γρήγορη ματιά που εμφανίζει τη διεύθυνση URL της τοποθεσίας][csurl]
    
        **OR**  
    
    * Εγκατάσταση και ρύθμιση παραμέτρων του [Azure Powershell](../powershell-install-configure.md)και, στη συνέχεια, χρησιμοποιήστε την ακόλουθη εντολή:
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Αποθηκεύστε το όνομα τομέα που χρησιμοποιείται στη διεύθυνση URL που επιστρέφονται από μία μεθόδους, όπως θα το χρειαστείτε κατά τη δημιουργία μια εγγραφή CNAME.

1.  Συνδεθείτε στην τοποθεσία Web του μητρώου καταχώρησης τομέων DNS και μεταβείτε στη σελίδα για τη Διαχείριση DNS. Αναζητήστε συνδέσεις ή περιοχές της τοποθεσίας με την ετικέτα ως **Το όνομα του τομέα**, **DNS**ή **Το όνομα διακομιστή διαχείρισης**.

2.  Εύρεση τώρα όπου μπορείτε να επιλέξετε ή να εισαγάγετε του CNAME. Ίσως χρειαστεί να επιλέξετε τον τύπο εγγραφής μείωση των προς τα κάτω ή να μεταβείτε σε μια σελίδα ρυθμίσεις για προχωρημένους. Θα πρέπει να μπορείτε να αναζητήσετε τις λέξεις **CNAME**, **ψευδώνυμο**ή **δευτερεύοντες τομείς**.

3.  Μπορείτε, επίσης, πρέπει να παρέχετε το ψευδώνυμο τομέα ή υποτομέα για την εγγραφή CNAME, όπως **www** εάν θέλετε να δημιουργήσετε ένα ψευδώνυμο για **www.customdomain.com**. Εάν θέλετε να δημιουργήσετε ένα ψευδώνυμο για τον ριζικό τομέα, μπορεί να αναφέρεται ως το '**@**' σύμβολο στα εργαλεία το μητρώο καταχώρησης ονομάτων DNS.

4. Στη συνέχεια, πρέπει να δώσετε ένα όνομα κανονικής κεντρικού υπολογιστή, το οποίο είναι τομέας **cloudapp.net** της εφαρμογής σας σε αυτήν την περίπτωση.

Για παράδειγμα, της παρακάτω εγγραφής CNAME προωθεί όλη την κυκλοφορία από **www.contoso.com** σε **contoso.cloudapp.net**, το προσαρμοσμένο όνομα τομέα της εφαρμογής σας ανάπτυξη:

| Όνομα/δευτερεύων τομέας ψευδωνύμου/κεντρικού υπολογιστή | Κανονικής τομέα     |
| ------------------------- | -------------------- |
| www                       | contoso.cloudapp.NET |

Επισκέπτη της **www.contoso.com** θα δείτε ποτέ τον κεντρικό υπολογιστή του true (contoso.cloudapp.net), ώστε η διαδικασία προώθησης είναι ορατή από τον τελικό χρήστη.

> [AZURE.NOTE]
> Το παραπάνω παράδειγμα ισχύει μόνο για την κίνηση στο ο δευτερεύων τομέας **www** . Επειδή δεν μπορείτε να χρησιμοποιήσετε χαρακτήρες μπαλαντέρ με τις εγγραφές CNAME, πρέπει να δημιουργήσετε μία εγγραφή CNAME για κάθε τομέα/δευτερεύων τομέας. Εάν θέλετε να την κατεύθυνση της κίνησης από δευτερεύοντες τομείς, όπως είναι οι \*. contoso.com, στη διεύθυνση cloudapp.net, μπορείτε να ρυθμίσετε τις παραμέτρους μιας καταχώρησης **Ανακατεύθυνση διεύθυνση URL** ή **Διεύθυνση URL προς τα εμπρός** στις ρυθμίσεις σας DNS ή να δημιουργήσετε μια εγγραφή A.


## <a name="add-an-a-record-for-your-custom-domain"></a>Προσθήκη μιας εγγραφής Α για τον προσαρμοσμένο τομέα σας

Για να δημιουργήσετε μια εγγραφή A, πρέπει πρώτα να βρουν την εικονική διεύθυνση IP από την υπηρεσία στο cloud. Στη συνέχεια, προσθέστε μια νέα καταχώρηση στον πίνακα DNS για τον προσαρμοσμένο τομέα σας, χρησιμοποιώντας τα εργαλεία που παρέχονται από το μητρώο καταχώρησης ονομάτων τομέων. Κάθε μητρώο καταχώρησης ονομάτων τομέων έχει μια παρόμοια αλλά ελαφρώς διαφορετικές μέθοδο που καθορίζει μια εγγραφή A, αλλά οι έννοιες είναι η ίδια.

1. Χρησιμοποιήστε μία από τις παρακάτω μεθόδους για να λάβετε τη διεύθυνση IP του την υπηρεσία cloud.
    
    * συνδεθείτε στην [πύλη Azure κλασική], επιλέξτε την υπηρεσία cloud, επιλέξτε **τον πίνακα εργαλείων**και, στη συνέχεια, βρείτε την καταχώρηση **διεύθυνση δημόσιας εικονικό IP (VIP)** στην ενότητα **γρήγορη ματιά** .
    
        ![ενότητα γρήγορη ματιά που εμφανίζει το VIP][vip]
    
        **OR**  
    
    * Εγκατάσταση και ρύθμιση παραμέτρων του [Azure Powershell](../powershell-install-configure.md)και, στη συνέχεια, χρησιμοποιήστε την ακόλουθη εντολή:
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Εάν έχετε πολλές τελικά σημεία που σχετίζεται με την υπηρεσία cloud, θα λάβετε πολλές γραμμές που περιέχουν τη διεύθυνση IP, αλλά όλες θα πρέπει να εμφανίζουν την ίδια διεύθυνση.
    
    Αποθηκεύστε τη διεύθυνση IP, όπως θα το χρειαστείτε κατά τη δημιουργία μιας εγγραφής Α.

1.  Συνδεθείτε στην τοποθεσία Web του μητρώου καταχώρησης τομέων DNS και μεταβείτε στη σελίδα για τη Διαχείριση DNS. Αναζητήστε συνδέσεις ή περιοχές της τοποθεσίας με την ετικέτα ως **Το όνομα του τομέα**, **DNS**ή **Το όνομα διακομιστή διαχείρισης**.

2.  Εύρεση τώρα όπου μπορείτε να επιλέξετε ή να εισαγάγετε μια εγγραφή. Ίσως χρειαστεί να επιλέξετε τον τύπο εγγραφής μείωση των προς τα κάτω ή να μεταβείτε σε μια σελίδα ρυθμίσεις για προχωρημένους.

3. Επιλέξτε ή πληκτρολογήστε τον τομέα ή δευτερεύων τομέας που θα χρησιμοποιήσετε αυτήν την εγγραφή Α. Για παράδειγμα, επιλέξτε **www** , εάν θέλετε να δημιουργήσετε ένα ψευδώνυμο για **www.customdomain.com**. Εάν θέλετε να δημιουργήσετε μια καταχώρηση μπαλαντέρ για όλους τους δευτερεύοντες τομείς, πληκτρολογήστε '__*__'. Αυτό θα καλύπτουν όλους τους δευτερεύοντες τομείς όπως **mail.customdomain.com**, **login.customdomain.com**και **www.customdomain.com**.

    Εάν θέλετε να δημιουργήσετε μια εγγραφή A για τον ριζικό τομέα, μπορεί να αναφέρεται ως το '**@**' σύμβολο στα εργαλεία το μητρώο καταχώρησης ονομάτων DNS.

4. Εισαγάγετε τη διεύθυνση IP του την υπηρεσία cloud στο πεδίο που παρέχεται. Αυτό συσχετίζει την καταχώρηση τομέα που χρησιμοποιείται σε της εγγραφής A με τη διεύθυνση IP της ανάπτυξης υπηρεσία cloud.

Για παράδειγμα, τα ακόλουθα εγγραφής προωθεί όλη την κυκλοφορία από **contoso.com** **137.135.70.239**, τη διεύθυνση IP της εφαρμογής σας ανάπτυξη:

| Όνομα κεντρικού υπολογιστή/δευτερεύων τομέας | Διεύθυνση IP     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



Αυτό το παράδειγμα δείχνει τη δημιουργία μιας εγγραφής Α για τον ριζικό τομέα. Εάν θέλετε να δημιουργήσετε μια καταχώρηση μπαλαντέρ για να καλύψετε όλα δευτερεύοντες τομείς, θα πρέπει να εισαγάγετε '__*__' ως ο δευτερεύων τομέας.

>[AZURE.WARNING]
>Διευθύνσεις IP στο Azure είναι δυναμικά από προεπιλογή. Που πιθανότατα θα θέλετε να χρησιμοποιήσετε μια [δεσμευμένη διεύθυνση IP](../virtual-network/virtual-networks-reserved-public-ip.md) για να βεβαιωθείτε ότι δεν αλλάζει τη διεύθυνση IP.

## <a name="next-steps"></a>Επόμενα βήματα

* [Πώς μπορείτε να διαχειριστείτε τις υπηρεσίες Cloud](cloud-services-how-to-manage.md)
* [Πώς μπορείτε να αντιστοιχίσετε CDN περιεχόμενο σε ένα προσαρμοσμένο τομέα](../cdn/cdn-map-content-to-custom-domain.md)
* [Γενικές ρύθμιση παραμέτρων για την υπηρεσία cloud](cloud-services-how-to-configure.md).
* Μάθετε πώς να [αναπτύξετε μια υπηρεσία στο cloud](cloud-services-how-to-create-deploy.md).
* Ρύθμιση παραμέτρων [πιστοποιητικά ssl](cloud-services-configure-ssl-certificate.md).




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure κλασική πύλη]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 