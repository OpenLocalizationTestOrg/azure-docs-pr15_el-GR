<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων Azure από Python | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το χώρο αποθήκευσης αρχείων Azure από Python για αποστολή, λίστα, λήψη και διαγραφή αρχείων."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων Azure από Python

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο θα σας δείξουν πώς να εκτελείτε συνηθισμένα σενάρια χρησιμοποιώντας χώρο αποθήκευσης αρχείων. Τα δείγματα είναι γραμμένες σε Python και χρησιμοποιήστε το [Microsoft Azure SDK χώρου αποθήκευσης για Python]. Τα σενάρια καλύπτεται περιλαμβάνουν αποστολή, την καταχώρηση, τη λήψη και διαγραφή αρχείων.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Δημιουργήστε ένα κοινόχρηστο στοιχείο

Το αντικείμενο **FileService** σάς επιτρέπει να εργάζεστε με κοινόχρηστα στοιχεία, καταλόγους και αρχεία. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **FileService** . Προσθέστε την ακόλουθη κοντά στο επάνω μέρος οποιουδήποτε αρχείου Python στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού αποθήκευσης Azure.

    from azure.storage.file import FileService

Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **FileService** χρησιμοποιώντας το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό.  Αντικατάσταση 'ο λογαριασμός μου' και 'mykey' με το όνομα του λογαριασμού και αριθμού-κλειδιού.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Στο παρακάτω παράδειγμα κώδικα, μπορείτε να χρησιμοποιήσετε ένα αντικείμενο **FileService** για να δημιουργήσετε το κοινόχρηστο στοιχείο, εάν δεν υπάρχει.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Αποστολή αρχείου σε ένα κοινόχρηστο στοιχείο

Ένα κοινόχρηστο χώρο αποθήκευσης Azure αρχείο περιέχει τουλάχιστον, έναν ριζικό κατάλογο όπου μπορεί να βρίσκονται τα αρχεία. Σε αυτήν την ενότητα, θα μάθετε πώς μπορείτε να αποστείλετε ένα αρχείο από τον τοπικό χώρο αποθήκευσης στον ριζικό κατάλογο της ένα κοινόχρηστο στοιχείο.

Για να δημιουργήσετε ένα αρχείο και αποστείλετε δεδομένων, χρησιμοποιήστε το **Δημιουργία\_αρχείο\_από\_διαδρομή**, **Δημιουργία\_αρχείο\_από\_ροή**, **Δημιουργία\_αρχείο\_από\_byte που** ή **Δημιουργία\_αρχείο\_από\_κειμένου** μεθόδους. Έχουν υψηλού επιπέδου μεθόδους που εκτελούν τα απαραίτητα chunking όταν το μέγεθος των δεδομένων που υπερβαίνει τα 64 MB.

**Δημιουργία\_αρχείο\_από\_διαδρομή** αποστέλλει τα περιεχόμενα του αρχείου από την καθορισμένη διαδρομή, και **Δημιουργία\_αρχείο\_από\_ροή** κάνει αποστολή των περιεχομένων από ένα ήδη ανοικτό αρχείο/ροή. **Δημιουργία\_αρχείο\_από\_byte που** αποστέλλει έναν πίνακα byte, και **Δημιουργία\_αρχείο\_από\_κειμένου** στέλνει την τιμή καθορισμένο κείμενο χρησιμοποιώντας την καθορισμένη κωδικοποίηση (η προεπιλογή είναι UTF-8).

Το παρακάτω παράδειγμα αποστέλλει τα περιεχόμενα του αρχείου **sunset.png** στο αρχείο **το_αρχείο_μου** .

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Πώς μπορείτε να: δημιουργία καταλόγου

Μπορείτε επίσης να οργανώσετε χώρου αποθήκευσης με την τοποθέτηση αρχείων μέσα δευτερευόντων καταλόγων αντί όλες τις στον ριζικό κατάλογο. Η υπηρεσία αποθήκευσης αρχείων Azure σας επιτρέπει να δημιουργήσετε όσες σε καταλόγους καθώς επιτρέπει το λογαριασμό σας. Ο παρακάτω κώδικας θα δημιουργήσει μια δευτερεύουσα κατάλογο με όνομα **sampledir** κάτω από το ριζικό κατάλογο.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Πώς μπορείτε να: λίστα αρχείων και καταλόγων σε ένα κοινόχρηστο στοιχείο

Για να δημιουργήσετε μια λίστα των αρχείων και καταλόγων σε ένα κοινόχρηστο στοιχείο, χρησιμοποιήστε το **λίστα\_σε καταλόγους\_και\_αρχεία** μέθοδο. Αυτή η μέθοδος επιστρέφει μια γεννήτρια. Ο ακόλουθος κώδικας εξάγει το **όνομα** κάθε αρχείου και καταλόγου σε ένα κοινόχρηστο στοιχείο στην κονσόλα.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Λήψη αρχείων

Για να κάνετε λήψη δεδομένων από ένα αρχείο, χρησιμοποιήστε **Λήψη\_αρχείο\_για να\_διαδρομή**, **Λήψη\_αρχείο\_για να\_ροή**, **γρήγορα\_αρχείο\_για να\_byte που**, ή **γρήγορα\_αρχείο\_για να\_κειμένου**. Έχουν υψηλού επιπέδου μεθόδους που εκτελούν τα απαραίτητα chunking όταν το μέγεθος των δεδομένων που υπερβαίνει τα 64 MB.

Το παρακάτω παράδειγμα παρουσιάζει τη χρήση **Λήψη\_αρχείο\_να\_διαδρομή** να κάνετε λήψη των περιεχομένων του αρχείου **το_αρχείο_μου** και αποθηκεύσετε το αρχείο **εκτός sunset.png** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Διαγραφή αρχείου

Τέλος, για να διαγράψετε ένα αρχείο, καλέστε **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρου αποθήκευσης αρχείων, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

- [Κέντρο για προγραμματιστές Python](/develop/python/)
- [Υπηρεσίες Azure αποθήκευσης REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]
- [Χώρος αποθήκευσης Microsoft Azure SDK για Python]

[Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
[Χώρος αποθήκευσης Microsoft Azure SDK για Python]: https://github.com/Azure/azure-storage-python
