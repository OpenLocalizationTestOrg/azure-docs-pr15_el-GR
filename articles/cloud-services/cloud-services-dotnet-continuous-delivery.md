<properties
    pageTitle="Συνεχής παράδοσης για cloud services με TFS στο Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε συνεχής παράδοσης για τις εφαρμογές του Azure cloud. Δείγματα κώδικα για MSBuild δηλώσεις της γραμμής εντολών και δεσμών ενεργειών του PowerShell."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Συνεχής παράδοσης για τις υπηρεσίες Cloud στο Azure

Τη διαδικασία που περιγράφεται σε αυτό το άρθρο σας δείχνει πώς να ρυθμίσετε συνεχή παράδοσης για τις εφαρμογές του Azure cloud. Αυτή η διαδικασία σάς επιτρέπει να αυτόματη δημιουργία πακέτων και ανάπτυξη του πακέτου σε Azure μετά από κάθε κώδικα μεταβίβασης ελέγχου. Η διαδικασία δημιουργίας πακέτου που περιγράφονται σε αυτό το άρθρο είναι ισοδύναμη με την εντολή **πακέτου** στο Visual Studio και τα βήματα δημοσίευσης είναι ισοδύναμο με την εντολή **Δημοσίευση** στο Visual Studio.
Το άρθρο περιγράφει τις μεθόδους που θα χρησιμοποιούσατε για να δημιουργήσετε ένα διακομιστή Δόμηση με MSBuild δηλώσεις της γραμμής εντολών και δεσμών ενεργειών του Windows PowerShell, και επίσης δείχνει πώς μπορείτε να ρυθμίσετε προαιρετικά Visual Studio ομάδας διακομιστή Foundation - Δημιουργία ομάδας ορισμούς για να χρησιμοποιήσετε τις εντολές MSBuild και δεσμών ενεργειών του PowerShell. Η διαδικασία είναι προσαρμόσιμες περιβάλλον δόμησης και περιβάλλοντα Azure προορισμού.

Μπορείτε επίσης να χρησιμοποιήσετε Visual Studio Team Services, μια έκδοση του TFS που φιλοξενείται στο Azure, να το κάνετε αυτό πιο εύκολα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Συνεχής παράδοση Azure με χρήση Visual Studio ομάδας των υπηρεσιών][].

Πριν ξεκινήσετε, πρέπει να μπορείτε να δημοσιεύσετε την εφαρμογή από το Visual Studio.
Αυτό θα εξασφαλίσει ότι όλοι οι πόροι είναι διαθέσιμες και προετοιμασία όταν επιχειρείτε να αυτοματοποιήσετε τη διαδικασία δημοσίευσης.

## <a name="1-configure-the-build-server"></a>1: ρύθμιση παραμέτρων του διακομιστή Δόμηση

Για να δημιουργήσετε ένα πακέτο Azure χρησιμοποιώντας MSBuild, πρέπει να εγκαταστήσετε το απαιτούμενο λογισμικό και εργαλεία στο διακομιστή Δόμηση.

Δεν απαιτείται να έχουν εγκατασταθεί στο διακομιστή Δόμηση Visual Studio. Εάν θέλετε να χρησιμοποιήσετε υπηρεσία Δόμηση ομάδας Foundation στη Διαχείριση διακομιστή Δόμηση, ακολουθήστε τις οδηγίες στην τεκμηρίωση των [Υπηρεσιών Δόμηση ομάδας Foundation][] .

1.  Στο διακομιστή Δόμηση, εγκαταστήστε το [.NET Framework 4.5.2][], το οποίο περιλαμβάνει το MSBuild.
2.  Εγκαταστήστε την πιο πρόσφατη [Εργαλεία σύνταξης του Azure για το .NET](https://azure.microsoft.com/develop/net/).
3.  Εγκαταστήστε τις [βιβλιοθήκες του Azure για .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Αντιγράψτε το αρχείο Microsoft.WebApplication.targets από μια εγκατάσταση του Visual Studio στο διακομιστή Δόμηση.

    Σε έναν υπολογιστή με εγκατεστημένο το Visual Studio, αυτό το αρχείο βρίσκεται στον κατάλογο C:\\Files(x86) πρόγραμμα\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Θα πρέπει να το αντιγράψετε στον ίδιο κατάλογο στο διακομιστή Δόμηση.
5.  Εγκαταστήστε το [Azure εργαλεία για το Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: δημιουργία ενός πακέτου χρησιμοποιώντας εντολές MSBuild

Αυτή η ενότητα περιγράφει τον τρόπο για να δημιουργήσετε μια εντολή MSBuild που δημιουργεί ένα πακέτο Azure. Εκτέλεση αυτού του βήματος στο διακομιστή Δόμηση για να επαληθεύσετε ότι όλα τα στοιχεία έχει ρυθμιστεί σωστά και ότι η εντολή MSBuild δεν τι θέλετε να κάνετε. Μπορείτε είτε να προσθέσετε αυτήν τη γραμμή εντολών για υπάρχουσες δέσμες ενεργειών Δόμηση στο διακομιστή Δόμηση ή μπορείτε να χρησιμοποιήσετε τη γραμμή εντολών σε έναν ορισμό που δημιουργείτε σε TFS, όπως περιγράφεται στην επόμενη ενότητα. Για περισσότερες πληροφορίες σχετικά με τις παραμέτρους γραμμής εντολών και MSBuild, ανατρέξτε στο θέμα [Αναφορά γραμμής εντολών MSBuild](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1.  Εάν είναι εγκατεστημένο Visual Studio στο διακομιστή Δόμηση, εντοπίστε και επιλέξτε **Visual Studio εντολή προτροπής που** στο φάκελο **Εργαλεία του Visual Studio** στα Windows.

    Εάν δεν είναι εγκατεστημένο Visual Studio στο διακομιστή Δόμηση, ανοίξτε μια γραμμή εντολών και βεβαιωθείτε ότι είναι δυνατή η πρόσβαση στη διαδρομή MSBuild.exe. MSBuild έχει εγκατασταθεί με το .NET Framework στη διαδρομή % WINDIR %\\Microsoft.NET\\Framework\\*έκδοση*. Για παράδειγμα, για να προσθέσετε τη μεταβλητή περιβάλλοντος PATH MSBuild.exe όταν έχετε εγκαταστήσει το .NET Framework 4, πληκτρολογήστε την παρακάτω εντολή στη γραμμή εντολών:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  Στη γραμμή εντολών, μεταβείτε στο φάκελο που περιέχει το αρχείο Azure έργου που θέλετε να δημιουργήσετε.

3.  Εκτέλεση MSBuild με το/Target: δημοσίευση επιλογή όπως στο ακόλουθο παράδειγμα:

        MSBuild /target:Publish

    Αυτή η επιλογή μπορεί να είναι συντετμημένη ως /t: δημοσίευση. Την επιλογή /t:Publish στο MSBuild δεν θα πρέπει να συγχέονται με τη δημοσίευση εντολές που είναι διαθέσιμες στο Visual Studio όταν έχετε το SDK Azure εγκατεστημένο. Το /t: δημοσίευση εκδόσεις μόνο την επιλογή του Azure πακέτων. Το αναπτύξετε τα πακέτα με τις εντολές δημοσίευση στο Visual Studio.

    Προαιρετικά, μπορείτε να καθορίσετε το όνομα του έργου ως παράμετρος MSBuild. Εάν δεν καθορίζεται, χρησιμοποιείται ο τρέχων κατάλογος. Για περισσότερες πληροφορίες σχετικά με τις επιλογές της γραμμής εντολών MSBuild, ανατρέξτε στο θέμα [Αναφορά γραμμής εντολών MSBuild](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

4.  Εντοπίστε το αποτέλεσμα. Από προεπιλογή, αυτή η εντολή δημιουργεί έναν κατάλογο σχέση με τον ριζικό φάκελο για το έργο, όπως *ProjectDir*\\Ανακύκλωσης\\*ρύθμισης παραμέτρων*\\app.publish\\. Όταν δημιουργείτε ένα Azure έργο, μπορείτε να δημιουργήσετε δύο αρχεία, το ίδιο το αρχείο πακέτου και συνοδευτικό αρχείο ρύθμισης παραμέτρων:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Από προεπιλογή, κάθε Azure έργο περιλαμβάνει ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας (αρχείο .cscfg) για τοπική εκδόσεις (εντοπισμού σφαλμάτων) και ένα άλλο για εκδόσεις cloud (οργάνωσης ή παραγωγής), αλλά μπορείτε να προσθέσετε ή να καταργήσετε αρχεία ρύθμισης παραμέτρων της υπηρεσίας, όπως απαιτείται. Όταν δημιουργείτε ένα πακέτο μέσα σε Visual Studio, θα σας ζητηθεί ποια υπηρεσία αρχείο ρύθμισης παραμέτρων για να συμπεριλάβετε μαζί με το πακέτο.

5.  Καθορίστε το αρχείο ρύθμισης παραμέτρων υπηρεσίας. Όταν δημιουργείτε ένα πακέτο με τη χρήση MSBuild, το αρχείο ρύθμισης παραμέτρων τοπική υπηρεσία περιλαμβάνεται από προεπιλογή. Για να συμπεριλάβετε ένα αρχείο ρύθμισης παραμέτρων διαφορετική υπηρεσία, ορίστε την ιδιότητα TargetProfile της εντολής MSBuild, όπως στο ακόλουθο παράδειγμα:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Καθορίστε τη θέση για το αποτέλεσμα. Ορίστε τη διαδρομή, χρησιμοποιώντας το /p:PublishDir =*Κατάλογος* \\ την επιλογή, όπως το τελικά διαχωριστικό ανάστροφη κάθετο, όπως στο ακόλουθο παράδειγμα:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Μόλις συνταχθεί και ελεγχθεί μια κατάλληλη γραμμή εντολών MSBuild για να δημιουργήσετε τα έργα σας και να συνδυάσετε δημιουργώντας ένα πακέτο Azure, μπορείτε να προσθέσετε αυτήν τη γραμμή εντολών για να τις δέσμες ενεργειών Δόμηση. Εάν ο διακομιστής Δόμηση χρησιμοποιεί προσαρμοσμένων δεσμών ενεργειών, αυτή η διαδικασία θα εξαρτώνται από τις λεπτομέρειες της διαδικασίας Δημιουργία προσαρμοσμένου. Εάν χρησιμοποιείτε TFS ως ένα περιβάλλον δόμησης, στη συνέχεια, μπορείτε να ακολουθήσετε τις οδηγίες στο επόμενο βήμα για να προσθέσετε τη Δόμηση Azure πακέτου σας διαδικασία δημιουργίας.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: δημιουργία ενός πακέτου με χρήση TFS Δημιουργία ομάδας

Εάν έχετε ομάδας Foundation διακομιστή (TFS) ρύθμιση ως έναν ελεγκτή Δόμηση και ο διακομιστής Δόμηση ρύθμιση ως μηχανή Δόμηση TFS και, στη συνέχεια, μπορείτε προαιρετικά να ρυθμίσετε ένα αυτοματοποιημένο build για το πακέτο Azure σας. Για πληροφορίες σχετικά με τη ρύθμιση και χρήση διακομιστή Foundation ομάδας ως σύστημα Δόμηση, ανατρέξτε στο θέμα [κλίμακα εκτός του συστήματος Δόμηση][]. Συγκεκριμένα, η παρακάτω διαδικασία προϋποθέτει ότι έχετε ρυθμίσει το διακομιστή Δόμηση όπως περιγράφεται στο [ανάπτυξης και ρύθμισης παραμέτρων ενός διακομιστή Δόμηση][], και ότι έχετε δημιουργήσει ένα έργο της ομάδας, δημιουργήσατε ένα έργο υπηρεσία cloud του έργου ομάδας.

Για να ρυθμίσετε τις παραμέτρους TFS για να δημιουργήσετε Azure πακέτων, εκτελέστε τα παρακάτω βήματα:

1.  Στο Visual Studio στον υπολογιστή σας ανάπτυξης, στο μενού Προβολή, επιλέξτε **Εξερεύνηση ομάδας**ή επιλέξτε το συνδυασμό πλήκτρων Ctrl +\\, Ctrl + M. Στο παράθυρο της Εξερεύνησης ομάδας, αναπτύξτε τον κόμβο **δημιουργεί** ή επιλέξτε τη σελίδα που **δημιουργεί** και, επιλέξτε **Νέο ορισμό δημιουργία**.

    ![Νέα επιλογή Δημιουργία ορισμού][0]

2.  Επιλέξτε την καρτέλα **έναυσμα** και ορίστε τις επιθυμητές προϋποθέσεις για όταν θέλετε να δημιουργηθεί το πακέτο. Για παράδειγμα, καθορίστε **Συνεχής ενοποίησης** για να δημιουργήσετε το πακέτο κάθε φορά μια προέλευση στοιχείου ελέγχου μεταβίβασης ελέγχου που προκύπτει.

3.  Επιλέξτε την καρτέλα **Ρυθμίσεις προέλευσης** και βεβαιωθείτε ότι το φάκελο του έργου σας εμφανίζεται στη στήλη **Προέλευσης στοιχείου ελέγχου φακέλου** και η κατάσταση είναι **ενεργό**.

4.  Επιλέξτε την καρτέλα **Δημιουργία προεπιλογών** και, στην περιοχή Δημιουργία ελεγκτή, επιβεβαιώστε το όνομα του διακομιστή Δόμηση.  Επίσης, ενεργοποιήστε την επιλογή **αντίγραφο δημιουργία εξόδου στον παρακάτω φάκελο απόθεσης** και καθορίστε τη θέση θέλετε απόθεσης.

5.  Επιλέξτε την καρτέλα **διεργασία** . Στην καρτέλα διεργασία, επιλέξτε το προεπιλεγμένο πρότυπο, στην περιοχή **Δημιουργία**, επιλέξτε το έργο, εάν δεν είναι ήδη επιλεγμένο, και αναπτύξτε την ενότητα **για προχωρημένους** στην ενότητα **Δημιουργία** του πλέγματος.

6.  Επιλέξτε **Ορίσματα MSBuild**και ορίστε την κατάλληλη ορίσματα MSBuild της γραμμής εντολών, όπως περιγράφεται στο βήμα 2 παραπάνω. Για παράδειγμα, πληκτρολογήστε **/t: δημοσίευση /p:PublishDir =\\\\myserver\\αποθέτει\\ ** για να δημιουργήσετε ένα πακέτο και να αντιγράψετε τα αρχεία του πακέτου στη θέση \\ \\myserver\\αποθέτει\\:

    ![Ορίσματα MSBuild][2]

    **Σημείωση:** Αντιγραφή των αρχείων σε κοινόχρηστο δημόσια διευκολύνει την ανάπτυξη με μη αυτόματο τρόπο τα πακέτα από τον υπολογιστή σας στην ανάπτυξη.

5.  Δοκιμάστε την επιτυχία της το βήμα Δόμηση, επιλέγοντας μια αλλαγή στο έργο σας ή στην ουρά τα μια νέα έκδοση. Στην ουρά του μια νέα έκδοση, από την Εξερεύνηση ομάδας, κάντε δεξί κλικ **Όλων των ορισμών δημιουργία,** και, στη συνέχεια, επιλέξτε **Δημιουργία νέου ουρά**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: δημοσιεύσετε ένα πακέτο χρησιμοποιώντας μια δέσμη ενεργειών PowerShell

Αυτή η ενότητα περιγράφει τον τρόπο για να δημιουργήσετε μια δέσμη ενεργειών του Windows PowerShell που θα δημοσιεύσετε το αποτέλεσμα του πακέτου εφαρμογής Cloud σε Azure χρησιμοποιώντας προαιρετικών παραμέτρων. Αυτήν τη δέσμη ενεργειών μπορεί να καλείται μετά τη Δόμηση βήμα στο σας automation Δημιουργία προσαρμοσμένου. Μπορεί να ονομάζεται επίσης από πρότυπο διεργασίας δραστηριότητες ροής εργασίας στο Visual Studio TFS ομάδας Δημιουργήστε.

1.  Εγκατάσταση τα [cmdlet του Azure PowerShell][] (v0.6.1 ή νεότερη έκδοση).
    Κατά τη φάση ρύθμισης cmdlet επιλέξτε για να εγκαταστήσετε το ως ένα συμπληρωματικό. Σημειώστε ότι αυτό επίσημα υποστηριζόμενη έκδοση αντικαθιστά την παλαιότερη έκδοση που παρέχεται μέσω CodePlex, παρόλο που οι προηγούμενες εκδόσεις έχουν αρίθμηση 2.x.x.

2.  Έναρξη PowerShell Azure χρησιμοποιώντας το μενού Έναρξη ή σελίδα έναρξης. Εάν ξεκινήσετε με αυτόν τον τρόπο, το cmdlet του Azure PowerShell θα φορτώνεται.

3.  Στη γραμμή εντολών του PowerShell, βεβαιωθείτε ότι τα cmdlet του PowerShell έχουν φορτωθεί, πληκτρολογώντας την εντολή μερικό `Get-Azure` και, στη συνέχεια, πατώντας το πλήκτρο Tab για συμπλήρωση δήλωσης.

    Εάν πατήσετε το πλήκτρο Tab επανειλημμένα, θα πρέπει να βλέπετε διάφορες εντολές του PowerShell Azure.

4.  Βεβαιωθείτε ότι μπορείτε να συνδεθείτε στη συνδρομή σας στο Azure με την εισαγωγή πληροφοριών τη συνδρομή σας από το αρχείο .publishsettings.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Στη συνέχεια, πληκτρολογήστε την εντολή

    `Get-AzureSubscription`

    Η επιλογή αυτή εμφανίζει πληροφορίες σχετικά με τη συνδρομή σας. Βεβαιωθείτε ότι όλα τα στοιχεία είναι σωστά.

4.  Αποθηκεύστε το πρότυπο δέσμης ενεργειών που παρέχονται στο τέλος αυτού του άρθρου στο φάκελο δεσμών ενεργειών ως c:\\δέσμες ενεργειών\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Εξετάστε την ενότητα τις παραμέτρους της δέσμης ενεργειών. Προσθέστε ή τροποποιήστε τις προεπιλεγμένες τιμές. Αυτές οι τιμές μπορεί να αντικατασταθεί πάντα περνώντας στο ρητή παραμέτρους.

6.  Βεβαιωθείτε ότι υπάρχει δημιουργούνται έγκυρη cloud λογαριασμούς υπηρεσίας και χώρο αποθήκευσης στη συνδρομή σας που απευθύνονται από τη δέσμη ενεργειών δημοσίευση. Για να αποστείλετε και να αποθηκεύσετε προσωρινά το αρχείο πακέτου και ρύθμισης παραμέτρων ανάπτυξης, ενώ δημιουργείται ανάπτυξης θα χρησιμοποιηθεί το λογαριασμό χώρου αποθήκευσης (χώρος αποθήκευσης αντικειμένων blob).

    -   Για να δημιουργήσετε μια νέα υπηρεσία cloud, να καλέσετε αυτήν τη δέσμη ενεργειών είτε να χρησιμοποιήσετε το [Azure κλασική πύλη](http://go.microsoft.com/fwlink/?LinkID=213885). Το όνομα της υπηρεσίας cloud θα χρησιμοποιηθεί ως πρόθεμα σε ένα πλήρως προσδιορισμένο όνομα τομέα και επομένως πρέπει να είναι μοναδικό.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Για να δημιουργήσετε ένα νέο λογαριασμό του χώρου αποθήκευσης, να καλέσετε αυτήν τη δέσμη ενεργειών είτε να χρησιμοποιήσετε το [Azure κλασική πύλη](http://go.microsoft.com/fwlink/?LinkID=213885). Το όνομα λογαριασμού του χώρου αποθήκευσης που θα χρησιμοποιηθεί ως πρόθεμα σε ένα πλήρως προσδιορισμένο όνομα τομέα και επομένως πρέπει να είναι μοναδικό. Μπορείτε να δοκιμάσετε χρησιμοποιώντας το ίδιο όνομα με την υπηρεσία cloud.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Κλήση της δέσμης ενεργειών απευθείας από το Azure PowerShell ή σύρματος του αυτήν τη δέσμη ενεργειών για τον αυτοματισμό Δόμηση κεντρικού υπολογιστή για να εκτελούνται μετά τη δημιουργία πακέτου.

    >[AZURE.IMPORTANT] Η δέσμη ενεργειών θα πάντα να διαγράψετε ή να αντικαταστήσετε το υπάρχον αναπτύξεις από προεπιλογή εάν εντοπίζονται. Αυτό είναι απαραίτητο για να ενεργοποιήσετε την συνεχής παράδοσης από αυτοματισμού όπου είναι δυνατή η χωρίς ερώτηση στο χρήστη.

    **Παράδειγμα σενάριο 1:** συνεχούς ανάπτυξης για το περιβάλλον δημιουργίας σταδίων μιας υπηρεσίας:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    Αυτό συνήθως ακολουθείται προς τα επάνω από δοκιμής επαλήθευση και μια κάνετε εναλλαγή VIP. Το αντιμετάθεσης VIP μπορεί να γίνει μέσω του [Azure κλασική πύλη](http://go.microsoft.com/fwlink/?LinkID=213885) ή χρησιμοποιώντας το cmdlet μετακίνηση ανάπτυξης.

    **Παράδειγμα σενάριο 2:** συνεχούς ανάπτυξης στο περιβάλλον παραγωγής της υπηρεσίας αποκλειστικό δοκιμής

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Σύνδεση απομακρυσμένης επιφάνειας εργασίας:**

    Εάν είναι ενεργοποιημένη η σύνδεση απομακρυσμένης επιφάνειας εργασίας στο έργο σας Azure που θα πρέπει να εκτελέσετε επιπλέον εφάπαξ βήματα για να βεβαιωθείτε ότι το σωστό πιστοποιητικό υπηρεσία Cloud αποστέλλεται σε όλες τις υπηρεσίες cloud στοχευμένες από αυτήν τη δέσμη ενεργειών.

    Εντοπίστε τις τιμές αποτύπωση πιστοποιητικού αναμένεται από ρόλους σας. Οι τιμές αποτύπωση είναι ορατή στην ενότητα πιστοποιητικά του αρχείου ρύθμισης παραμέτρων cloud (δηλαδή ServiceConfiguration.Cloud.cscfg). Επίσης, είναι ορατή στο παράθυρο διαλόγου Ρύθμιση παραμέτρων απομακρυσμένης επιφάνειας εργασίας στο Visual Studio όταν εμφανίζετε επιλογές και προβολή του επιλεγμένου πιστοποιητικού.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Αποστολή πιστοποιητικά απομακρυσμένης επιφάνειας εργασίας ως εφάπαξ ρύθμισης βήμα χρησιμοποιώντας την ακόλουθη δέσμη ενεργειών cmdlet:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Για παράδειγμα:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    Εναλλακτικά μπορείτε να εξαγάγετε το αρχείο πιστοποιητικού PFX με ιδιωτικό κλειδί και να αποστείλετε τα πιστοποιητικά σε κάθε υπηρεσία cloud προορισμού με την [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885). 
    Διαβάστε το ακόλουθο άρθρο για να μάθετε περισσότερα: [[http://msdn.microsoft.com/library/windowsazure/gg443832.aspx]].

    **Αναβάθμιση ανάπτυξης ανάπτυξης έναντι Διαγραφή -\> νέας ανάπτυξης**

    Η δέσμη ενεργειών θα από προεπιλογή να εκτελέσει μια ανάπτυξη αναβάθμιση ($enableDeploymentUpgrade = 1) όταν η παράμετρος δεν μεταβιβάζεται στο ή μεταβιβάζεται ρητά την τιμή 1. Για μεμονωμένες εμφανίσεις αυτό έχει το πλεονέκτημα της διαρκεί λιγότερο χρόνο από μια πλήρη ανάπτυξη. Για τις εμφανίσεις που απαιτούν υψηλή διαθεσιμότητα αυτό διαθέτει επίσης το πλεονέκτημα της κλείσετε ορισμένα παρουσιών που εκτελούνται ενώ άλλα άτομα έχουν αναβαθμιστεί (παρουσίαση μερικών τομέα σας ενημέρωση), καθώς και VIP σας δεν θα διαγραφούν.

    Αναβάθμιση ανάπτυξης μπορεί να απενεργοποιηθεί στη δέσμη ενεργειών ($enableDeploymentUpgrade = 0) ή από μεταβίβαση *- enableDeploymentUpgrade 0* ως μια παράμετρο, που αλλάζει τη συμπεριφορά δέσμη ενεργειών για να πρώτα να διαγράψετε οποιοδήποτε υπάρχον ανάπτυξης και, στη συνέχεια, δημιουργήστε μια νέα ανάπτυξη.

    >[AZURE.IMPORTANT] Η δέσμη ενεργειών θα πάντα να διαγράψετε ή να αντικαταστήσετε το υπάρχον αναπτύξεις από προεπιλογή εάν εντοπίζονται. Αυτό είναι απαραίτητο για να ενεργοποιήσετε την συνεχής παράδοσης από αυτοματισμού όπου είναι δυνατή η χωρίς ερώτηση χρήστη/χειριστή.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: δημοσιεύσετε ένα πακέτο χρησιμοποιώντας τη Δόμηση ομάδας TFS

Αυτό το προαιρετικό βήμα συνδέεται TFS ομάδας δημιουργία στη δέσμη ενεργειών που δημιουργήσατε στο βήμα 4, που χειρίζεται δημοσίευσης από τη δημιουργία πακέτου για Azure. Αυτό πρέπει να πραγματοποιηθούν τροποποίηση του προτύπου διαδικασίας που χρησιμοποιείται από τον ορισμό Δόμηση ώστε να εκτελείται μια δραστηριότητα δημοσίευση στο τέλος της ροής εργασίας. Η δημοσίευση δραστηριότητα θα εκτελέσει την εντολή PowerShell που περνά μέσα στις παραμέτρους από το build. Έξοδος από το MSBuild ως προορισμό και δημοσίευση δέσμης ενεργειών θα να διοχετευθούν σε το αποτέλεσμα τυπική Δόμηση.

1.  Επεξεργαστείτε τον ορισμό Δόμηση υπεύθυνοι για συνεχή ανάπτυξη.

2.  Επιλέξτε την καρτέλα **διεργασία** .

3.  Ακολουθήστε [αυτές τις οδηγίες](http://msdn.microsoft.com/library/dd647551.aspx) για να προσθέσετε ένα έργο δραστηριότητας για το πρότυπο διεργασίας Δόμηση, κάντε λήψη του προεπιλεγμένου προτύπου, προσθέστε στο έργο και μεταβίβαση ελέγχου. Δώστε στο πρότυπο διεργασίας Δόμηση ένα νέο όνομα, όπως AzureBuildProcessTemplate.

3.  Επιστρέψτε στην καρτέλα **διεργασία** και χρησιμοποιήστε **Εμφάνιση λεπτομερειών** για να εμφανίσετε μια λίστα με Δόμηση διαθέσιμα πρότυπα διεργασίας. Επιλέξτε το κουμπί **Δημιουργία...** και μεταβείτε στο έργο που μόλις προσθέσατε και μεταβίβαση ελέγχου του. Εντοπίστε το πρότυπο που μόλις δημιουργήσατε και επιλέξτε **OK**.

4.  Ανοίξτε το επιλεγμένο πρότυπο διεργασίας για επεξεργασία. Μπορείτε να ανοίξετε απευθείας στη Σχεδίαση ροής εργασίας ή στο πρόγραμμα επεξεργασίας XML ώστε να λειτουργεί με το XAML.

5.  Προσθέστε την παρακάτω λίστα ορισμάτων νέα ως ξεχωριστά στοιχεία γραμμής στην καρτέλα ορίσματα της σχεδίασης ροής εργασίας. Όλα τα ορίσματα πρέπει να έχει την κατεύθυνση = σε και πληκτρολογήστε = συμβολοσειρά. Αυτά θα χρησιμοποιηθεί για παραμέτρους ροής από τον ορισμό Δόμηση σε τη ροή εργασίας, που, στη συνέχεια, να χρησιμοποιηθούν για να καλέσετε τη δέσμη ενεργειών δημοσίευση.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Λίστα ορισμάτων][3]

    Το αντίστοιχο XAML μοιάζει κάπως έτσι:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Προσθέστε μια νέα σειρά στο τέλος της εκτέλεσης στον παράγοντα:

    1.  Ξεκινήστε προσθέτοντας μια πρόταση εάν δραστηριότητα για να ελέγξετε για ένα αρχείο έγκυρη δέσμης ενεργειών. Ορίστε τη συνθήκη σε αυτήν την τιμή:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  Σε έπειτα την περίπτωση, της δήλωσης εάν, προσθέστε μια νέα δραστηριότητα ακολουθίας. Καθορισμός του εμφανιζόμενου ονόματος 'Έναρξη δημοσίευση'

    3.  Με την έναρξη δημοσίευση ακολουθίας επιλεγμένο, προσθέστε την ακόλουθη λίστα νέες μεταβλητές ως ξεχωριστά στοιχεία γραμμής στην καρτέλα μεταβλητές του προγράμματος σχεδίασης ροής εργασίας. Όλες τις μεταβλητές θα πρέπει να έχει τύπο μεταβλητής = συμβολοσειρά και το εύρος = Έναρξη δημοσίευση. Αυτά θα χρησιμοποιηθεί για παραμέτρους ροής από τον ορισμό Δόμηση σε τη ροή εργασίας, που, στη συνέχεια, να χρησιμοποιηθούν για να καλέσετε τη δέσμη ενεργειών δημοσίευση.

        -   SubscriptionDataFilePath, τύπου συμβολοσειράς

        -   PublishScriptFilePath, τύπου συμβολοσειράς

            ![Νέες μεταβλητές][4]

    4.  Εάν χρησιμοποιείτε TFS 2012 ή προηγούμενες εκδόσεις, Προσθήκη δραστηριότητας ConvertWorkspaceItem στην αρχή της νέας σειράς. Εάν χρησιμοποιείτε TFS 2013 ή νεότερη έκδοση, προσθέστε μια δραστηριότητα GetLocalPath στην αρχή της νέας σειράς. Για μια ConvertWorkspaceItem, ορίστε τις ιδιότητες ως εξής: κατεύθυνση = ServerToLocal, εμφανιζόμενο όνομα = 'Μετατροπή δημοσιεύσετε filename δέσμης ενεργειών', εισαγωγής = 'PublishScriptLocation', αποτέλεσμα = 'PublishScriptFilePath', χώρος εργασίας = 'Χώρος εργασίας'. Για μια δραστηριότητα GetLocalPath, ορίστε την ιδιότητα IncomingPath για να 'PublishScriptLocation' και το αποτέλεσμα σε 'PublishScriptFilePath'. Αυτήν τη δραστηριότητα μετατρέπει τη διαδρομή προς τη δέσμη ενεργειών δημοσίευση από TFS διακομιστή θέσεις (εάν υπάρχει) σε μια διαδρομή τυπική στον τοπικό δίσκο.

    5.  Εάν χρησιμοποιείτε TFS 2012 ή προηγούμενες εκδόσεις, προσθέστε μια άλλη δραστηριότητα ConvertWorkspaceItem στο τέλος της νέας σειράς. Κατεύθυνση = ServerToLocal, εμφανιζόμενο όνομα = 'Μετατροπή συνδρομή filename', εισαγωγής = 'SubscriptionDataFileLocation', αποτέλεσμα = 'SubscriptionDataFilePath', χώρος εργασίας = 'Χώρος εργασίας'. Εάν χρησιμοποιείτε TFS 2013 ή νεότερη έκδοση, προσθέστε ένα άλλο GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation', και αποτέλεσμα = 'SubscriptionDataFilePath'.

    6.  Προσθήκη μιας δραστηριότητας InvokeProcess στο τέλος της νέας σειράς.
        Αυτή η δραστηριότητα κλήσεις PowerShell.exe με τα ορίσματα που δόθηκαν στο από τον ορισμό Δημιουργία.

        1.  Ορίσματα = String.Format ("-αρχείο""{0}" "- όνομα_υπηρεσίας {1} - storageAccountName {2} - packageLocation""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6}-περιβάλλον""{7}" "", PublishScriptFilePath, όνομα_υπηρεσίας, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, περιβάλλον)

        2.  Εμφανιζόμενο όνομα = Execute δημοσίευση δέσμης ενεργειών

        3.  Όνομα αρχείου = "PowerShell" (περιλαμβάνει των εισαγωγικών)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  Στο πλαίσιο κειμένου ενότητα **Χειρισμού τυπική έξοδο** από το InvokeProcess, ορίστε την τιμή πλαισίου κειμένου για να 'δεδομένα'. Αυτή είναι μια μεταβλητή για να αποθηκεύσετε τα δεδομένα εξόδου τυπική.

    8.  Προσθήκη δραστηριότητας WriteBuildMessage ακριβώς κάτω από την ενότητα **Χειρισμού τυπικής εξόδου** . Ορίσετε τη σπουδαιότητα = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' και το μήνυμα = 'δεδομένα'. Αυτό εξασφαλίζει ότι θα λάβετε εγγραφή στην τυπική έξοδο της δέσμης ενεργειών στο αποτέλεσμα Δόμηση.

    9.  Στο πλαίσιο κειμένου ενότητα **Χειρισμού σφαλμάτων εξόδου** από το InvokeProcess, ορίστε την τιμή πλαισίου κειμένου για να 'δεδομένα'. Αυτή είναι μια μεταβλητή για να αποθηκεύσετε τα δεδομένα του τυπικού σφάλματος.

    10. Προσθήκη δραστηριότητας WriteBuildError ακριβώς κάτω από την ενότητα **Χειρισμού σφαλμάτων εξόδου** . Ορισμός του μηνύματος = 'δεδομένα'. Αυτό εξασφαλίζει ότι θα λάβετε εγγραφή τα σφάλματα τυπική της δέσμης ενεργειών στο αποτέλεσμα σφάλμα Δόμηση.

    11. Διορθώστε τυχόν σφάλματα, που επισημαίνεται με μπλε θαυμαστικό σημάδια. Τοποθετήστε το δείκτη επάνω από τα σημάδια θαυμαστικό για να λάβετε μια υπόδειξη σχετικά με το σφάλμα. Αποθηκεύστε τη ροή εργασίας για να καταργήσετε σφάλματα.

    Το τελικό αποτέλεσμα της οι δραστηριότητες ροής εργασίας "Δημοσίευση" θα μοιάζει κάπως έτσι στη σχεδίαση:

    ![Δραστηριότητες ροής εργασίας][5]

    Το τελικό αποτέλεσμα της οι δραστηριότητες ροής εργασίας "Δημοσίευση" θα μοιάζει κάπως έτσι σε XAML:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Αποθήκευση η ροή εργασίας πρότυπο διεργασίας Δόμηση και μεταβίβαση ελέγχου του αρχείου.

8.  Επεξεργαστείτε τον ορισμό Δόμηση (κλείστε το, εάν είναι ήδη ανοικτό), και επιλέξτε το κουμπί **Δημιουργία** Εάν ακόμη δεν βλέπετε το νέο πρότυπο στη λίστα των προτύπων διεργασίας.

9.  Ορίστε την παράμετρο ιδιότητα τιμές στην ενότητα διάφορα ως εξής:

    1.  CloudConfigLocation ='c:\\αποθέτει\\app.publish\\ServiceConfiguration.Cloud.cscfg' *προέρχεται από αυτήν την τιμή: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = ' c:\\αποθέτει\\app.publish\\ContactManager.Azure.cspkg' *προέρχεται από αυτήν την τιμή: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = ' c:\\δέσμες ενεργειών\\WindowsAzure\\PublishCloudService.ps1'

    4.  Όνομα_υπηρεσίας = 'mycloudservicename' *Χρήση εδώ το όνομα της υπηρεσίας κατάλληλο cloud*

    5.  Περιβάλλον = 'Ενδιάμεσου σταδίου'

    6.  StorageAccountName = 'mystorageaccountname' *χρήση το χώρο αποθήκευσης στο κατάλληλο όνομα λογαριασμού του εδώ*

    7.  SubscriptionDataFileLocation = ' c:\\δέσμες ενεργειών\\WindowsAzure\\Subscription.xml'

    8.  SubscriptionName = "Προεπιλογή"

    ![Τιμές παραμέτρων ιδιοτήτων][6]

10. Αποθηκεύστε τις αλλαγές στον ορισμό Δημιουργία.

11. Ουρά μια Δόμηση για να εκτελέσετε Δημιουργία πακέτου και δημοσίευση. Εάν έχετε ένα έναυσμα οριστεί σε συνεχή ενοποίησης, θα μπορείτε να εκτελέσετε αυτήν τη συμπεριφορά σε κάθε μεταβίβασης ελέγχου.

### <a name="publishcloudserviceps1-script-template"></a>Πρότυπο PublishCloudService.ps1 δέσμης ενεργειών

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να ενεργοποιήσετε απομακρυσμένου εντοπισμού σφαλμάτων κατά τη χρήση συνεχής παράδοσης, ανατρέξτε στο θέμα [Ενεργοποίηση της απομακρυσμένης εντοπισμού κατά τη χρήση συνεχής παράδοσης για να δημοσιεύσετε στο Azure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Συνεχής παράδοση Azure χρησιμοποιώντας τις υπηρεσίες ομάδας Visual Studio]: cloud-services-continuous-delivery-use-vso.md  
  [Υπηρεσία Δόμηση Foundation ομάδας]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [Κλιμάκωση εκτός του συστήματος Δόμηση]: https://msdn.microsoft.com/library/dd793166.aspx
  [Ανάπτυξη και ρύθμιση παραμέτρων διακομιστή Δόμηση]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure cmdlet του PowerShell]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
