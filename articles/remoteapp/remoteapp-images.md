<properties
    pageTitle="Τι περιέχει τις εικόνες των προτύπων Azure RemoteApp; | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τις εικόνες των προτύπων περιλαμβάνεται με το Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Τι περιέχει τις εικόνες των προτύπων Azure RemoteApp;

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Συνδρομή σας Azure RemoteApp περιλαμβάνει τρεις εικόνες των προτύπων:


- Windows Server 2012
- Microsoft Office 365 ProPlus (συνδρομή στο Office 365 απαιτούνται)
- Microsoft Office 2013 Professional Plus (δοκιμαστική έκδοση μόνο)

> [AZURE.IMPORTANT]Τη συνδρομή σας στο Azure RemoteApp εκχωρεί πρόσβαση σε του λογισμικού σε εικόνες, με την εξαίρεση Office 365 ProPlus, το οποίο απαιτεί ξεχωριστή συνδρομή, και Office 2013, το οποίο δεν είναι δυνατό να χρησιμοποιηθούν σε παραγωγής. Αυτό σημαίνει ότι μπορείτε να μοιραστείτε τα προγράμματα ή εφαρμογές σε τις εικόνες των προτύπων με τους χρήστες σας. Για παράδειγμα, εάν δημιουργήσετε μια συλλογή που χρησιμοποιεί την εικόνα του Windows Server 2012 R2, μπορείτε να δημοσιεύσετε συστήματος κέντρο Endpoint Protection για τους χρήστες για να αποκτήσετε πρόσβαση μέσω της εφαρμογής RemoteApp.
>
> Δείτε τις [Λεπτομέρειες άδειας χρήσης RemoteApp](remoteapp-licensing.md) για περισσότερες πληροφορίες. Και [Χρήση του Office με το Azure RemoteApp](remoteapp-o365.md) για το Office παραχώρησης πολλαπλών αδειών χρήσης πληροφορίες.

Διαβάστε παρακάτω για λεπτομέρειες σχετικά με τι περιέχει κάθε εικόνα.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("η βανίλιας εικόνα")
Αυτή η εικόνα είναι που βασίζεται σε Microsoft Windows Server 2012 R2 κέντρο δεδομένων λειτουργικό σύστημα και έχει τους εξής ρόλους και δυνατότητες που έχουν εγκατασταθεί για να πληροί τις απαιτήσεις για εικόνες των προτύπων Azure RemoteApp:


- .NET framework διαίρεσης 4,5, 3.5.1, 3.5
- Εμπειρία στην επιφάνεια εργασίας
- Υπηρεσίες γραφής και χειρογράφου
- Υποδομής πολυμέσων
- Host απομακρυσμένη περίοδο λειτουργίας επιφάνειας εργασίας
- 4.0 του Windows PowerShell
- Windows PowerShell ISE
- Υποστήριξη WoW64

Αυτή η εικόνα έχει επίσης τις παρακάτω εφαρμογές που εγκαταστήσατε:

- Adobe Flash Player
- Το Microsoft Silverlight
- Microsoft συστήματος κέντρο 2012 Endpoint Protection
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (απαιτείται συνδρομή)
Το Office 365 είναι στην πιο ζητήθηκε εφαρμογή, ώστε να που δημιουργήσαμε μια εικόνα "Προσαρμογή" για να εργαστείτε με.

Αυτή η εικόνα είναι μια επέκταση της βανίλιας εικόνας και περιλαμβάνει τα παρακάτω στοιχεία του Microsoft Office 365 ProPlus εγκατεστημένο εκτός από τα στοιχεία που περιγράφονται στην εικόνα Windows Server 2012 R2:


- Πρόσβαση
- Excel
- Lync
- Το OneNote
- OneDrive για επιχειρήσεις (Σημειώστε ότι ο συγχρονισμός παράγοντας δεν υποστηρίζεται για χρήση με το Azure RemoteApp)
- Το Outlook
- Το PowerPoint
- Το Word
- Εργαλεία γλωσσικού ελέγχου του Microsoft Office

Η εικόνα περιλαμβάνει επίσης Visio Pro και το Project Pro.

Και τις παρακάτω εφαρμογές, καθώς και:

- SQL Native client
- Πρόγραμμα οδήγησης ODBC
- SQL Server δεδομένων εξορυκτικού προγράμματος-πελάτη
- MasterDataServices προγράμματος-πελάτη
- Microsoft Publisher
- PowerQuery
- PowerMap


Πλήρη λειτουργικότητα του Office 365 ProPlus εφαρμογές είναι διαθέσιμη μόνο για τους χρήστες που έχουν ένα πρόγραμμα του Office 365 ProPlus. Για περισσότερες λεπτομέρειες σχετικά με το Office 365 συνδρομητικά προγράμματα ανατρέξτε στο θέμα [προγράμματα υπηρεσίας του Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Υπάρχουν ακόμη ερωτήσεις; Ανατρέξτε στις πληροφορίες του [Office 365 + RemoteApp](remoteapp-o365.md) . Δείτε επίσης το νέο άρθρο, [πώς μπορείτε να χρησιμοποιήσετε με το Azure RemoteApp συνδρομή στο Office 365](remoteapp-officesubscription.md).

Σημειώστε ότι πρέπει να αδειών χρήσης του Office 365 ProPlus, το Visio Pro και του Project Pro ξεχωριστά - κάθε έχουν τις δικές τους άδεια χρήσης.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (δοκιμαστική έκδοση μόνο)
Κατά τη διάρκεια της δοκιμαστικής περιόδου, μπορείτε να δοκιμάσετε την υπηρεσία με την εικόνα του Office 2013.

Αυτή η εικόνα είναι μια επέκταση της βανίλιας εικόνας και περιλαμβάνει τα παρακάτω στοιχεία του Microsoft Office 2013 Professional Plus εγκατεστημένο εκτός από τα στοιχεία που περιγράφονται στην εικόνα Windows Server 2012 R2:


- Πρόσβαση
- Excel
- Lync
- Το OneNote
- OneDrive για επιχειρήσεις (Σημειώστε ότι ο συγχρονισμός παράγοντας δεν υποστηρίζεται για χρήση με το Azure RemoteApp)
- Το Outlook
- Το PowerPoint
- Έργο
- Το Visio
- Το Word
- Εργαλεία γλωσσικού ελέγχου του Microsoft Office

> [AZURE.IMPORTANT]**Νομικές πληροφορίες:** Αυτή η εικόνα δεν περιλαμβάνει μια άδεια χρήσης του Microsoft Office και *δεν μπορούν να χρησιμοποιηθούν για την παραγωγή*. Εικόνα προορίζεται μόνο για χρήση δοκιμαστικής έκδοσης του Office 2013 Professional Plus. Εάν θέλετε να χρησιμοποιήσετε εφαρμογές του Office στο Azure RemoteApp για παραγωγής, πρέπει να χρησιμοποιήσετε το Office 365 ProPlus εικόνα. Για περισσότερες λεπτομέρειες σχετικά με το Office αδειών χρήσης, ανατρέξτε στο θέμα [χρήση του Office 365 με Azure RemoteApp](remoteapp-o365.md)
