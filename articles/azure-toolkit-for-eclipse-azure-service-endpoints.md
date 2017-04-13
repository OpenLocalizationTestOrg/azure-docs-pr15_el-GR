<properties
    pageTitle="Τα τελικά σημεία Azure υπηρεσίας"
    description="Περιγράφει τις ρυθμίσεις τελικού σημείου υπηρεσίας Azure στην εργαλειοθήκη του Azure για Έκλειψη."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Τα τελικά σημεία Azure υπηρεσίας #

Τα τελικά σημεία Azure service καθορίζουν εάν η εφαρμογή σας είναι αναπτυχθεί σε και ελέγχονται από την καθολική Azure πλατφόρμα, Azure με διαχείριση από 21Vianet στην Κίνα, ή μια ιδιωτική Azure πλατφόρμας. Το παράθυρο διαλόγου **Υπηρεσία τελικά σημεία** σάς επιτρέπει να καθορίσετε ποια τελικά σημεία υπηρεσίας που θέλετε να χρησιμοποιήσετε. Για να ανοίξετε το παράθυρο διαλόγου **Τελικά σημεία υπηρεσίας** , μέσα σε Έκλειψη, κάντε κλικ στην επιλογή **παράθυρο**, κάντε κλικ στην επιλογή **Προτιμήσεις**, αναπτύξτε το στοιχείο **Azure**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπηρεσία τελικά σημεία**. Το πεδίο **Ενεργό ρύθμιση** καθορίζει ποιες τελικά σημεία Azure service θα χρησιμοποιηθεί για τα έργα Azure στο τρέχον χώρο εργασίας σας.

Η ακόλουθη εικόνα δείχνει το παράθυρο διαλόγου **Υπηρεσία τελικά σημεία** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Για να ορίσετε τα τελικά σημεία υπηρεσίας ##

Στο παράθυρο διαλόγου **Τελικά σημεία υπηρεσίας** , εκτελέστε μία από τις εξής ενέργειες:

* Εάν θέλετε να χρησιμοποιήσετε την καθολική Azure πλατφόρμα, από την αναπτυσσόμενη λίστα **Ενεργού ρύθμιση** , επιλέξτε **windowsazure.com** και κάντε κλικ στο κουμπί **OK**.
* Εάν θέλετε να χρησιμοποιήσετε Azure με διαχείριση από 21Vianet στην Κίνα, από την αναπτυσσόμενη λίστα **Ενεργού ρύθμιση** , επιλέξτε **windowsazure.cn (Κίνα)** και κάντε κλικ στο κουμπί **OK**.
* Εάν θέλετε να χρησιμοποιήσετε μια ιδιωτική Azure πλατφόρμα:
    1. Κάντε κλικ στην επιλογή **Επεξεργασία**.
    2. Ανοίγει ένα παράθυρο διαλόγου, που σας ενημερώνει ότι θα κλείσει το παράθυρο διαλόγου **Υπηρεσία τελικά σημεία** , και θα ανοίξει το αρχείο σύνολα προτίμηση. Κάντε κλικ στο **κουμπί OK**.
    3. Στο αρχείο preferencesets.xml, δημιουργήστε μια νέα `preferenceset` στοιχείο. Για αυτό το νέο στοιχείο, δημιουργήστε `name`, `blob`, `management`, `portalURL` και `publishsettings` χαρακτηριστικά, και προσθέστε τιμές για αυτές που αντιστοιχούν σε ιδιωτική Azure πλατφόρμα σας. Μπορείτε να χρησιμοποιήσετε τις τιμές που παρέχονται για την υπάρχουσα `preferenceset` στοιχεία ως πρότυπα. **Σημείωση**: Η τιμή που χρησιμοποιείται για την `blob` χαρακτηριστικού πρέπει να περιέχει το κείμενο "blob" στη διεύθυνση URL.
    4. Αποθηκεύστε και κλείστε preferencesets.xml.
    5. Ανοίξτε ξανά το παράθυρο διαλόγου **Υπηρεσία τελικά σημεία** .
    6. Από την αναπτυσσόμενη λίστα **Ενεργού ρύθμιση** , επιλέξτε το ενεργό σύνολο που δημιουργήσατε και κάντε κλικ στο κουμπί **OK**.
    7. Αφού δημιουργήσετε την πλατφόρμα σας ιδιωτικό Azure `preferenceset` στοιχείο, μπορείτε να αλλάξετε τις τιμές που έχουν εκχωρηθεί σε αυτήν, κάνοντας κλικ στο κουμπί **Επεξεργασία** στο παράθυρο διαλόγου **Υπηρεσίες τελικού σημείου** . Μπορείτε επίσης να δημιουργήσετε πολλές ιδιωτικό Azure πλατφόρμα `preferenceset` στοιχεία, εάν θέλετε.

## <a name="see-also"></a>Δείτε επίσης ##

[Azure Κιτ εργαλείων για Έκλειψη][]

[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη][] 

[Δημιουργία εφαρμογής κόσμο Hello για Azure στο Έκλειψη][]

Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure με Java, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure Java][].

<!-- URL List -->

[Κέντρο για προγραμματιστές του Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Κιτ εργαλείων για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkID=699529
[Δημιουργία εφαρμογής κόσμο Hello για Azure στο Έκλειψη]: http://go.microsoft.com/fwlink/?LinkID=699533
[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png