<properties 
   pageTitle="Προσθήκη μιας Azure Active Directory χρησιμοποιώντας συνδεδεμένες υπηρεσίες στο Visual Studio | Microsoft Azure"
   description="Προσθέστε μια Azure Active Directory χρησιμοποιώντας το παράθυρο διαλόγου Visual Studio προσθέσετε συνδεδεμένες υπηρεσίες"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Προσθήκη μιας Azure Active Directory χρησιμοποιώντας συνδεδεμένες υπηρεσίες στο Visual Studio 

##<a name="overview"></a>Επισκόπηση
Με τη χρήση Azure Active Directory (Azure AD), που μπορούν να υποστηρίξουν καθολική σύνδεση (SSO) για τις εφαρμογές web ASP.NET MVC ή τον έλεγχο ταυτότητας AD στις υπηρεσίες Web API. Με έλεγχο ταυτότητας Azure AD, οι χρήστες σας να χρησιμοποιήσετε τους λογαριασμούς από το Azure AD για να συνδεθείτε με τις εφαρμογές web. Τα πλεονεκτήματα της Azure AD τον έλεγχο ταυτότητας με το API Web περιλαμβάνουν δεδομένα βελτιωμένη ασφάλεια όταν εκθέσετε API από μια εφαρμογή web. Με το Azure AD, δεν χρειάζεται να διαχειριστείτε ένα σύστημα ξεχωριστή τον έλεγχο ταυτότητας με το δικό της διαχείρισης λογαριασμού και χρήστη.

## <a name="supported-project-types"></a>Οι τύποι υποστηρίζονται έργου

Μπορείτε να χρησιμοποιήσετε το παράθυρο διαλόγου συνδεδεμένες υπηρεσίες για να συνδεθείτε με Azure AD στους παρακάτω τύπους έργου.

- Έργα ASP.NET MVC

- Έργα ASP.NET Web API


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Σύνδεση με Azure AD χρησιμοποιώντας το παράθυρο διαλόγου συνδεδεμένες υπηρεσίες

1. Βεβαιωθείτε ότι έχετε ένα λογαριασμό Azure. Εάν δεν έχετε λογαριασμό Azure, μπορείτε να εγγραφείτε για μια [δωρεάν δοκιμαστική έκδοση](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Στο Visual Studio, ανοίξτε το μενού συντόμευσης του κόμβου **αναφορές** στο έργο σας και επιλέξτε **Προσθήκη συνδεδεμένες υπηρεσίες**.
1. Επιλέξτε **Azure AD ελέγχου ταυτότητας** και, στη συνέχεια, επιλέξτε **Ρύθμιση παραμέτρων**.

    ![Επιλέξτε Προσθήκη Azure AD ελέγχου ταυτότητας](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Στην πρώτη σελίδα του **Azure ρύθμιση παραμέτρων ελέγχου ταυτότητας AD**, επιλέξτε **Ρύθμιση παραμέτρων Καθολικής σύνδεσης με χρήση Azure AD**.

    Εάν το έργο σας έχει ρυθμιστεί με μια άλλη ρύθμιση παραμέτρων ελέγχου ταυτότητας, ο οδηγός σάς προειδοποιεί ότι συνεχίζουν απενεργοποιούν την προηγούμενη ρύθμιση παραμέτρων.

    ![Ρύθμιση παραμέτρων Azure AD στον Οδηγό](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Στη δεύτερη σελίδα, επιλέξτε έναν τομέα από την αναπτυσσόμενη λίστα **Domain** . Λίστα των τομέων περιέχει όλους τους τομείς προσβάσιμο από λογαριασμούς που παρατίθενται στο παράθυρο διαλόγου Ρυθμίσεις λογαριασμού. Ως εναλλακτική λύση, μπορείτε να εισαγάγετε ένα όνομα τομέα, εάν δεν μπορείτε να βρείτε αυτό που αναζητάτε, όπως mydomain.onmicrosoft.com. Μπορείτε να επιλέξετε την επιλογή για να δημιουργήσετε μια νέα εφαρμογή Azure AD ή χρησιμοποιήστε τις ρυθμίσεις από μια υπάρχουσα εφαρμογή Azure AD. 

    ![Ρύθμιση παραμέτρων Azure AD στον Οδηγό](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Στην τρίτη σελίδα του οδηγού, βεβαιωθείτε ότι είναι επιλεγμένο το πλαίσιο ελέγχου **ανάγνωση δεδομένων καταλόγου** . Ο οδηγός θα συμπληρώσουν το **μυστικό προγράμματος-πελάτη**. 

    ![Ρύθμιση παραμέτρων Azure AD στον Οδηγό](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Επιλέξτε το κουμπί " **Τέλος** ". Το παράθυρο διαλόγου προσθέτει τον κωδικό ρύθμισης παραμέτρων είναι απαραίτητο και αναφορές για να ενεργοποιήσετε το έργο σας για τον έλεγχο ταυτότητας Azure AD. Μπορείτε να δείτε τον τομέα AD στην [πύλη του Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Διαβάστε τη σελίδα γρήγορα αποτελέσματα που εμφανίζεται στο πρόγραμμα περιήγησης για ιδέες σχετικά με τα επόμενα βήματα και, στη σελίδα τι συνέβη για να δείτε πώς το έργο σας έχει τροποποιηθεί. Εάν θέλετε να ελέγξετε ότι όλα τα στοιχεία ολοκληρώθηκε με επιτυχία, ανοίξτε ένα από τα αρχεία έχουν τροποποιηθεί ρύθμισης παραμέτρων και επιβεβαιώστε ότι οι ρυθμίσεις που αναφέρονται σε τι συνέβη είναι εκεί. Για παράδειγμα, το κύριο web.config σε ένα έργο ASP.NET MVC θα έχετε αυτές τις ρυθμίσεις που προσθέσατε:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Πώς έχει τροποποιηθεί το έργο σας

Κατά την εκτέλεση του οδηγού, Visual Studio προσθέτει Azure AD και σχετικές αναφορές στο έργο σας. Για να προσθέσετε υποστήριξη για Azure AD επίσης τροποποίησης αρχείων ρύθμισης παραμέτρων και αρχεία κώδικα στο έργο σας. Οι συγκεκριμένες τροποποιήσεις που το κάνει Visual Studio εξαρτώνται από τον τύπο του έργου. Για λεπτομερείς πληροφορίες σχετικά με τον τρόπο τροποποιούνται ASP.NET MVC έργα, ανατρέξτε στο θέμα [Τι συνέβη – MVC έργα](http://go.microsoft.com/fwlink/p/?LinkID=513809). Για τα έργα Web API, ανατρέξτε στο θέμα [Τι συνέβη – έργα API Web](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Επόμενα βήματα

Υποβάλετε ερωτήσεις και να λάβετε βοήθεια.

 - [Φόρουμ MSDN: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Τεκμηρίωση Azure AD](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Καταχώρηση ιστολογίου: Εισαγωγή στο Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

