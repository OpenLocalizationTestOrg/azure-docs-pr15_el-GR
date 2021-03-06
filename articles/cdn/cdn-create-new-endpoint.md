<properties
     pageTitle="Χρήση του Azure CDN | Microsoft Azure"
     description="Αυτό το θέμα δείχνει πώς μπορείτε να ενεργοποιήσετε το δίκτυο περιεχομένου παράδοσης (CDN) για το Azure. Το πρόγραμμα εκμάθησης σας καθοδηγεί μέσω της δημιουργίας ενός νέου προφίλ CDN και τελικού σημείου."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Χρήση του Azure CDN  

Αυτό το θέμα καθοδηγεί σε Ενεργοποίηση Azure CDN, δημιουργώντας ένα νέο προφίλ CDN και τελικού σημείου.

>[AZURE.IMPORTANT] Για μια εισαγωγή σχετικά με τον τρόπο λειτουργίας CDN, καθώς και μια λίστα των δυνατοτήτων, ανατρέξτε στο θέμα η [Επισκόπηση CDN](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Δημιουργήστε ένα νέο προφίλ CDN

Ένα προφίλ CDN είναι μια συλλογή από τα τελικά σημεία CDN.  Κάθε προφίλ περιέχει μία ή περισσότερες τελικά σημεία CDN.  Ενδέχεται να θέλετε να χρησιμοποιήσετε πολλά προφίλ για να οργανώσετε τα τελικά σημεία σας CDN ανά τομέα internet, εφαρμογή web ή άλλα κριτήρια.

> [AZURE.NOTE] Από προεπιλογή, μια μεμονωμένη συνδρομή Azure περιορίζεται στα οκτώ CDN προφίλ. Κάθε προφίλ CDN περιορίζεται σε δέκα CDN τελικά σημεία.
>
> Τις τιμές CDN εφαρμόζεται στο επίπεδο CDN προφίλ. Εάν θέλετε να χρησιμοποιήσετε ένα μείγμα από τις πληροφορίες τιμολόγησης σειρές Azure CDN, θα χρειαστεί πολλών CDN προφίλ.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Δημιουργήστε ένα νέο τελικό σημείο CDN

**Για να δημιουργήσετε ένα νέο τελικό σημείο CDN**

1. Στην [Πύλη του Azure](https://portal.azure.com), μεταβείτε στο προφίλ σας CDN.  Που μπορεί να έχουν καρφιτσωμένη την στον πίνακα εργαλείων στο προηγούμενο βήμα.  Εάν που δεν, μπορείτε να το βρείτε, κάνοντας κλικ στην επιλογή **Αναζήτηση**, στη συνέχεια, **CDN προφίλ**και, κάνοντας κλικ στην επιλογή το προφίλ που σκοπεύετε να το τελικό σημείο για να προσθέσετε.

    Εμφανίζεται το blade CDN προφίλ.

    ![CDN προφίλ][cdn-profile-settings]

2. Κάντε κλικ στο κουμπί **Προσθήκη τελικού σημείου** .

    ![Προσθήκη κουμπιού τελικού σημείου][cdn-new-endpoint-button]

    Εμφανίζεται το blade **Προσθήκη τελικού σημείου** .

    ![Προσθήκη blade τελικού σημείου][cdn-add-endpoint]

3. Πληκτρολογήστε ένα **όνομα** για αυτό το τελικό σημείο CDN.  Αυτό το όνομα που θα χρησιμοποιηθεί για να αποκτήσετε πρόσβαση στο cache τους πόρους σας στον τομέα `<endpointname>.azureedge.net`.

4. Στην αναπτυσσόμενη λίστα **Τύπος προέλευσης** , επιλέξτε τον τύπο προέλευσης.  Επιλέξτε **χώρου αποθήκευσης** για ένα λογαριασμό αποθήκευσης Azure, **υπηρεσία στο Cloud** για μια υπηρεσία Cloud Azure, **Web App** για μια εφαρμογή Web Azure, ή **προσαρμοσμένο origin** για κάθε άλλης δυνατότητα δημόσιας πρόσβασης web διακομιστή προέλευσης (φιλοξενείται στο Azure ή σε κάποιο άλλο σημείο).

    ![Τύπος προέλευσης CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. Στην αναπτυσσόμενη λίστα **Origin hostname** , επιλέξτε ή πληκτρολογήστε τον τομέα σας origin.  Στην αναπτυσσόμενη λίστα θα εμφανιστούν όλες τις διαθέσιμες προελεύσεις του τύπου που καθορίσατε στο βήμα 4.  Εάν έχετε επιλέξει *origin προσαρμοσμένη* ως **Πληκτρολογήστε Origin**σας, θα πληκτρολογήστε στον τομέα σας προσαρμοσμένης προέλευσης.

6. Στο πλαίσιο κειμένου **Origin διαδρομή** , πληκτρολογήστε τη διαδρομή για τους πόρους που θέλετε για προσωρινή ή αφήστε κενό για να επιτρέψετε σε cache οποιονδήποτε πόρο στον τομέα που καθορίσατε στο βήμα 5.

7. Στην **κεφαλίδα του κεντρικού υπολογιστή προέλευσης**, πληκτρολογήστε την κεφαλίδα κεντρικού υπολογιστή που θέλετε το CDN για να στείλετε με κάθε αίτηση ή αποχώρηση από την προεπιλεγμένη.

    > [AZURE.WARNING] Ορισμένοι τύποι προελεύσεων, όπως το χώρο αποθήκευσης Azure και εφαρμογές Web, απαιτείται η κεφαλίδα κεντρικού υπολογιστή για να ταιριάζουν με τον τομέα της προέλευσης. Εάν δεν έχετε μια προέλευση που απαιτεί μια κεφαλίδα κεντρικού υπολογιστή διαφορετικό από τον τομέα, πρέπει να αφήσετε την προεπιλεγμένη τιμή.

8. Για το **πρωτόκολλο** και τη **θύρα προέλευσης**, καθορίστε τα πρωτόκολλα και τις θύρες που χρησιμοποιούνται για την πρόσβαση στους πόρους σας κατά την προέλευση.  Τουλάχιστον μία protocol (HTTP ή HTTPS) πρέπει να είναι επιλεγμένο.
    
    > [AZURE.NOTE] Τη **θύρα προέλευσης** επηρεάζει μόνο τι port το χρησιμοποιεί τελικό σημείο για να ανακτήσετε πληροφορίες από την προέλευση.  Το τελικό σημείο ίδια θα είναι διαθέσιμη μόνο για προγράμματα-πελάτες λήξης από την προεπιλογή HTTP και HTTPS θύρες (80 και 443), ανεξάρτητα από τη **θύρα προέλευσης**.  
    >
    > Τα τελικά σημεία **CDN Azure από Akamai** να μην επιτρέπεται το πλήρες εύρος θύρας TCP για προελεύσεις.  Για μια λίστα των θυρών προέλευσης που δεν επιτρέπονται, ανατρέξτε στο θέμα [CDN Azure από Akamai επιτρεπόμενων Origin θύρες](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Πρόσβαση σε CDN περιεχόμενο χρησιμοποιώντας HTTPS περιλαμβάνει τους ακόλουθους περιορισμούς:
    > 
    > - Πρέπει να χρησιμοποιήσετε το πιστοποιητικό SSL που παρέχεται από το CDN. Πιστοποιητικά άλλων κατασκευαστών δεν υποστηρίζονται.
    > - Πρέπει να χρησιμοποιήσετε τον τομέα που παρέχονται από CDN (`<endpointname>.azureedge.net`) σε access HTTPS περιεχόμενο. Υποστήριξη HTTPS δεν είναι διαθέσιμη για προσαρμοσμένα ονόματα τομέα (CNAMEs) επειδή το CDN δεν υποστηρίζει προσαρμοσμένο πιστοποιητικά αυτήν τη στιγμή.

9. Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε το νέο τελικό σημείο.

10. Αφού δημιουργηθεί το τελικό σημείο, εμφανίζεται σε μια λίστα με τα τελικά σημεία για το προφίλ. Η προβολή λίστας εμφανίζει τη διεύθυνση URL για να χρησιμοποιήσετε για να αποκτήσετε πρόσβαση στο cache περιεχομένου, καθώς και τον τομέα προέλευσης.

    ![Τελικό σημείο CDN][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Το τελικό σημείο δεν θα αμέσως είναι διαθέσιμη για χρήση, όπως ο χρόνος για την καταχώρηση για μεταδοθούν έως το CDN.  Για τα προφίλ <b>CDN Azure από Akamai</b> , η μετάδοση θα ολοκληρωθεί συνήθως μέσα σε ένα λεπτό.  Για τα προφίλ <b>CDN Azure από Verizon</b> , η μετάδοση συνήθως θα ολοκληρωθεί εντός 90 λεπτά, αλλά σε ορισμένες περιπτώσεις μπορεί να διαρκέσει περισσότερο.
    >    
    > Οι χρήστες που προσπαθούν να χρησιμοποιήσετε το όνομα τομέα CDN πριν από τη ρύθμιση παραμέτρων ορίου έχει μεταδοθούν σε το εμφανίζεται ένα θα λάβουν κώδικες απόκρισης HTTP 404.  Εάν έχουν περάσει μερικές ώρες, εφόσον έχετε δημιουργήσει το τελικό σημείο και εξακολουθείτε να λαμβάνετε απαντήσεις 404, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων CDN τα τελικά σημεία επιστρέφοντας 404 καταστάσεις](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Δείτε επίσης
- [Τον έλεγχο σε cache συμπεριφοράς των αιτήσεων με συμβολοσειρές ερωτήματος](cdn-query-string.md)
- [Πώς μπορείτε να αντιστοιχίσετε CDN περιεχόμενο σε ένα προσαρμοσμένο τομέα](cdn-map-content-to-custom-domain.md)
- [Προ-φόρτωση περιουσιακών στοιχείων σε ένα τελικό σημείο Azure CDN](cdn-preload-endpoint.md)
- [Εκκαθάριση Azure CDN τελικού σημείου](cdn-purge-endpoint.md)
- [Αντιμετώπιση προβλημάτων CDN τα τελικά σημεία επιστρέφοντας 404 καταστάσεις](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
