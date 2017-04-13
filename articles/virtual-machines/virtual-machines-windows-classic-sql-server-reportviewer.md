<properties 
    pageTitle="Χρήση ReportViewer σε μια τοποθεσία Web | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει τον τρόπο για να δημιουργήσετε μια τοποθεσία Web του Microsoft Azure με το στοιχείο ελέγχου Visual Studio ReportViewer που εμφανίζει μια αναφορά που είναι αποθηκευμένη σε ένα Microsoft Azure εικονική μηχανή."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Χρήση ReportViewer σε μια τοποθεσία Web που φιλοξενείται στο Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Μπορείτε να δημιουργήσετε μια τοποθεσία Web του Microsoft Azure με το στοιχείο ελέγχου Visual Studio ReportViewer που εμφανίζει μια αναφορά που είναι αποθηκευμένη σε ένα Microsoft Azure εικονική μηχανή. Το στοιχείο ελέγχου ReportViewer είναι σε μια εφαρμογή Web που δημιουργείτε χρησιμοποιώντας το πρότυπο εφαρμογής ASP.NET Web.

>[AZURE.IMPORTANT] Τα πρότυπα εφαρμογών Web ASP.NET MVC δεν υποστηρίζουν το στοιχείο ελέγχου ReportViewer.

Για να ενσωματώσετε ReportViewer στην τοποθεσία Microsoft Azure Web, πρέπει να ολοκληρώσετε τις παρακάτω εργασίες.

- **Προσθήκη** Συγκροτήσεις στο πακέτο ανάπτυξης

- **Ρύθμιση παραμέτρων** Έλεγχος ταυτότητας και εξουσιοδότηση

- **Δημοσίευση** της εφαρμογής ASP.NET Web σε Azure

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Διαβάστε την ενότητα "Γενικές προτάσεων και βέλτιστες πρακτικές" στο [SQL Server επιχειρηματικής ευφυΐας σε εικονικές μηχανές Windows Azure](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] Στοιχεία ελέγχου ReportViewer που αποστέλλονται με Visual Studio, Standard Edition ή νεότερη έκδοση. Εάν χρησιμοποιείτε το Web για προγραμματιστές Express Edition, πρέπει να εγκαταστήσετε το [Χρόνου ΕΚΤΈΛΕΣΗΣ 2012 πρόγραμμα ΠΡΟΒΟΛΉΣ του MICROSOFT ΑΝΑΦΟΡΆΣ](https://www.microsoft.com/download/details.aspx?id=35747) για να χρησιμοποιήσετε τις δυνατότητες χρόνου εκτέλεσης ReportViewer.
>
>ReportViewer έχει ρυθμιστεί σε κατάσταση λειτουργίας επεξεργασίας τοπικής δεν υποστηρίζεται στο Microsoft Azure.

Διαβάστε τη λευκή βίβλο [στοιχείο ελέγχου προγράμματος προβολής αναφοράς υπηρεσιών αναφοράς του Microsoft Azure εικονική μηχανή που βασίζεται σε και διακομιστές αναφορών](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx).

## <a name="adding-assemblies-to-the-deployment-package"></a>Προσθήκη συγκροτήσεις στο πακέτο ανάπτυξης

Όταν φιλοξενείτε το ASP.NET εφαρμογή εσωτερική εγκατάσταση, οι συγκροτήσεις ReportViewer εγκαθίστανται συνήθως απευθείας στο καθολικό cache συγκροτήσεων (GAC) του διακομιστή των υπηρεσιών IIS κατά τη διάρκεια της εγκατάστασης του Visual Studio και είναι δυνατή η πρόσβαση απευθείας από την εφαρμογή. Ωστόσο, όταν φιλοξενείτε εφαρμογής ASP.NET στο cloud, Microsoft Azure δεν επιτρέπει κάτι να έχει εγκατασταθεί σε GAC, ώστε να πρέπει να βεβαιωθείτε ότι οι συγκροτήσεις ReportViewer είναι διαθέσιμα τοπικά για την εφαρμογή σας. Μπορείτε να κάνετε αυτό, προσθέτοντας αναφορές τους στο έργο σας και να ρυθμίσετε τις παραμέτρους τους να αντιγραφούν τοπικά.

Στη λειτουργία Απομακρυσμένη επεξεργασία, το στοιχείο ελέγχου ReportViewer χρησιμοποιεί τις ακόλουθες συγκροτήσεις:

- **Microsoft.ReportViewer.WebForms.dll**: περιέχει τον κώδικα ReportViewer, το οποίο πρέπει να χρησιμοποιήσετε ReportViewer στη σελίδα σας. Μια αναφορά για αυτής της συγκρότησης προστίθεται στο έργο σας κατά την απόρριψη ενός στοιχείου ελέγχου ReportViewer σε μια σελίδα ASP.NET στο έργο σας.

- **Microsoft.ReportViewer.Common.dll**: περιέχει κλάσεις που χρησιμοποιούνται από το στοιχείο ελέγχου ReportViewer κατά το χρόνο εκτέλεσης. Δεν προστίθεται αυτόματα στο έργο σας.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Για να προσθέσετε μια αναφορά σε Microsoft.ReportViewer.Common

- Κάντε δεξιό κλικ κόμβο **αναφορές** του έργου σας και επιλέξτε **Προσθήκη αναφορά**, επιλέξτε συγκρότησης στην καρτέλα .NET και κάντε κλικ στο κουμπί **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Για να κάνετε τις συγκροτήσεις τοπικά προσβάσιμο από την εφαρμογή σας ASP.NET

1. Στο φάκελο " **αναφορές** ", κάντε κλικ στην επιλογή συγκρότησης Microsoft.ReportViewer.Common, έτσι ώστε να εμφανίζονται στο παράθυρο Ιδιότητες τις ιδιότητές του.

1. Στο παράθυρο ιδιοτήτων, ορίστε **Τοπικού αντιγράφου** στην τιμή True.

1. Επαναλάβετε τα βήματα 1 και 2 για Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Για να αποκτήσετε ένα πακέτο γλώσσας ReportViewer

1. Εγκαταστήστε το κατάλληλο πακέτο με δυνατότητα ανακατανομής χρόνου εκτέλεσης 2012 πρόγραμμα προβολής του Microsoft αναφοράς από το [Κέντρο λήψης της Microsoft](http://go.microsoft.com/fwlink/?LinkId=317386).

1. Επιλέξτε τη γλώσσα από την αναπτυσσόμενη λίστα και λαμβάνει ανακατεύθυνση της σελίδας στην αντίστοιχη σελίδα Κέντρο λήψης.

1. Κάντε κλικ στην επιλογή **λήψη** για να ξεκινήσετε τη λήψη του ReportViewerLP.exe.

1. Μετά τη λήψη ReportViewerLP.exe, κάντε κλικ στο κουμπί **Εκτέλεση** για να εγκαταστήσετε αμέσως, ή κάντε κλικ στο κουμπί **Αποθήκευση** για να το αποθηκεύσετε στον υπολογιστή σας. Εάν κάνετε κλικ στο κουμπί **Αποθήκευση**, να θυμάστε το όνομα του φακέλου όπου μπορείτε να αποθηκεύσετε το αρχείο.

1. Εντοπίστε το φάκελο όπου αποθηκεύσατε το αρχείο. Κάντε δεξί κλικ ReportViewerLP.exe, κάντε κλικ στην επιλογή **Εκτέλεση ως διαχειριστής**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι**.

1. Μετά την εκτέλεση ReportViewerLP.exe, θα δείτε το c:\windows\assembly έχει τον πόρο αρχείων **Microsoft.ReportViewer.Webforms.Resources** και **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Για να ρυθμίσετε τις παραμέτρους για τις μεταφρασμένα ελέγχου ReportViewer

1. Κάντε λήψη και εγκατάσταση του πακέτου με δυνατότητα ανακατανομής χρόνου εκτέλεσης 2012 πρόγραμμα προβολής του Microsoft αναφοράς, ακολουθώντας τις οδηγίες παραπάνω καθορισμένο.

1. Δημιουργία <language> φακέλου στο project και αντιγραφή της συγκρότησης σχετικό πόρος αρχεία εκεί. Τα αρχεία συγκρότησης πόρων που θα αντιγραφούν είναι: **Microsoft.ReportViewer.Webforms.Resources.dll** και **Microsoft.ReportViewer.Common.Resources.dll**. Επιλέξτε τα αρχεία συγκρότησης πόρων και, στο παράθυρο ιδιοτήτων, ορίστε **Αντιγραφή κατάλογο εξόδου** για να "**Αντιγραφή πάντα**".

1. Ορίστε το κουλτούρας & UICulture για το project web. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να ορίσετε τις τοπικές ρυθμίσεις και τοπικές ρυθμίσεις περιβάλλοντος εργασίας Χρήστη για μια σελίδα ASP.NET Web, ανατρέξτε στο θέμα [Τρόπος: Ορίστε την τοπικές ρυθμίσεις και τις τοπικές ρυθμίσεις του περιβάλλοντος εργασίας Χρήστη για διεθνοποίηση σελίδα Web ASP.NET](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Ρύθμιση παραμέτρων ελέγχου ταυτότητας και εξουσιοδότησης

Το ReportViewer πρέπει να χρησιμοποιήσετε τα σωστά διαπιστευτήρια για τον έλεγχο ταυτότητας με το διακομιστή αναφορών και τα διαπιστευτήρια πρέπει να επιτρέπεται από το διακομιστή αναφορών για να αποκτήσετε πρόσβαση τις αναφορές που θέλετε. Για πληροφορίες σχετικά με τον έλεγχο ταυτότητας, ανατρέξτε στη λευκή βίβλο [στοιχείο ελέγχου προγράμματος προβολής αναφοράς υπηρεσιών αναφοράς του Microsoft Azure εικονική μηχανή που βασίζεται σε και διακομιστές αναφορών](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Δημοσίευση της εφαρμογής ASP.NET Web σε Azure

Για οδηγίες σχετικά με την δημοσίευση μιας εφαρμογής ASP.NET Web να Azure, ανατρέξτε στο θέμα [Τρόπος: μετεγκατάσταση και να δημοσιεύσετε μια εφαρμογή Web για Azure από το Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) και [Γρήγορα αποτελέσματα με το Web Apps και ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Εάν η εντολή Προσθήκη Azure ανάπτυξη του έργου ή προσθήκη Azure Cloud υπηρεσίας δεν εμφανίζεται στο μενού συντόμευσης στην Εξερεύνηση λύσεων, ίσως χρειαστεί να αλλάξετε το πλαίσιο προορισμού για το έργο στο .NET Framework 4.
>
>Οι δύο εντολές παρέχουν ουσιαστικά την ίδια λειτουργικότητα. Μία ή την εντολή "άλλα" θα εμφανίζεται στο μενού συντόμευσης ανάλογα με την έκδοση του Microsoft Azure SDK που έχετε εγκαταστήσει.

## <a name="resources"></a>Πόροι

[Αναφορές της Microsoft](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server επιχειρηματικής ευφυΐας σε εικονικές μηχανές Windows Azure](virtual-machines-windows-classic-ps-sql-bi.md)

[Χρήση του PowerShell για να δημιουργήσετε μια Εικονική Azure με ένα διακομιστή αναφοράς εγγενή λειτουργία](virtual-machines-windows-classic-ps-sql-report.md)

[Αναφορά υπηρεσιών αναφοράς πρόγραμμα προβολής του στοιχείου ελέγχου και Microsoft Azure εικονική μηχανή βάσει διακομιστές αναφορών](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
