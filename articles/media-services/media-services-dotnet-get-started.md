<properties
    pageTitle="Γρήγορα αποτελέσματα με την παράδοση περιεχομένου σε ζήτηση χρησιμοποιώντας .NET | Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί στα βήματα της εφαρμογής στην αίτηση παράδοσης περιεχομένου απαιτήσεων με υπηρεσίες πολυμέσων Azure χρησιμοποιώντας .NET."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Γρήγορα αποτελέσματα με την παράδοση περιεχομένου ζήτηση χρησιμοποιώντας .NET SDK

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Επισκόπηση 

Αυτό το πρόγραμμα εκμάθησης σας καθοδηγεί στη διαδικασία εφαρμογής μιας εφαρμογής παράδοσης περιεχομένου Video-on-Demand (VoD) με χρήση του Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) SDK για .NET.


Το πρόγραμμα εκμάθησης παρουσιάζει τη βασική ροή εργασίας Media Services και τα πιο συνηθισμένα προγραμματισμού αντικείμενα και εργασίες που απαιτούνται για την ανάπτυξη Media Services. Κατά την ολοκλήρωση του προγράμματος εκμάθησης, θα μπορείτε να μεταφέρετε με ροή ή σταδιακά λήψη ενός δείγματος αρχείου πολυμέσων που έχουν αποσταλεί, κωδικοποιημένη και λήψη.

## <a name="what-youll-learn"></a>Τι θα μάθετε

Το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες:

1.  Δημιουργήστε ένα λογαριασμό Media Services (με την πύλη Azure).
2.  Ρύθμιση παραμέτρων ροής τελικού σημείου (με την πύλη Azure).
3.  Δημιουργία και ρύθμιση παραμέτρων ένα έργο Visual Studio.
5.  Σύνδεση με το λογαριασμό Media Services.
6.  Δημιουργήστε ένα νέο πάγιο και στείλτε ένα αρχείο βίντεο.
7.  Κωδικοποίηση του αρχείου προέλευσης σε ένα σύνολο αρχείων MP4 προσαρμόσιμη ρυθμό μετάδοσης bit.
8.  Δημοσίευση του περιουσιακού στοιχείου και να μεταβαίνετε διευθύνσεις URL για τη ροή και προοδευτική λήψη.
9.  Δοκιμή με την αναπαραγωγή του περιεχομένου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Απαιτούνται τα εξής για να ολοκληρώσετε το πρόγραμμα εκμάθησης.

- Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. 
    
    Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](/pricing/free-trial/?WT.mc_id=A261C142F). Λαμβάνετε πιστώσεων που μπορούν να χρησιμοποιηθούν για να δοκιμάσετε την πληρωμή Azure υπηρεσίες. Ακόμα και μετά το πιστώσεων χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και να χρησιμοποιήσετε δωρεάν Azure υπηρεσίες και τις δυνατότητες, όπως η δυνατότητα Web Apps στο Azure εφαρμογής υπηρεσίας.
- Λειτουργικά συστήματα: Windows 8 ή νεότερη έκδοση, Windows 2008 R2, Windows 7.
- .NET framework 4.0 ή νεότερη έκδοση
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate ή Express) ή νεότερες εκδόσεις.


##<a name="download-sample"></a>Λήψη δείγματος

Λήψη και να εκτελέσετε ένα δείγμα από [εδώ](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Δημιουργήστε ένα λογαριασμό των υπηρεσιών Azure Media Services χρησιμοποιώντας την πύλη του Azure

Τα βήματα σε αυτήν την ενότητα δείχνουν πώς μπορείτε να δημιουργήσετε ένα λογαριασμό αθροιστικής μέτρησης ενισχύσεων.

1. Συνδεθείτε στην [πύλη του Azure](https://portal.azure.com/).
2. Κάντε κλικ στο κουμπί **+ νέο** > **πολυμέσων + CDN** > **υπηρεσίες πολυμέσων**.

    ![Δημιουργία υπηρεσίες πολυμέσων](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **ΔΗΜΙΟΥΡΓΊΑ** λογαριασμού ΥΠΗΡΕΣΊΕΣ ΠΟΛΥΜΈΣΩΝ εισαγάγετε απαιτούμενες τιμές.

    ![Δημιουργία υπηρεσίες πολυμέσων](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Στο πεδίο **Όνομα λογαριασμού**, πληκτρολογήστε το όνομα του νέου λογαριασμού αθροιστικής μέτρησης ενισχύσεων. Ένα όνομα λογαριασμού Media Services είναι όλα τα πεζά αριθμούς ή τα γράμματα χωρίς κενά διαστήματα και είναι 3 έως 24 χαρακτήρες.
    2. Στο συνδρομή, επιλέξετε από τις διαφορετικές συνδρομές Azure που έχετε πρόσβαση.
    
    2. Στην **Ομάδα πόρων**, επιλέξτε τον πόρο νέο ή υπάρχον.  Η ομάδα πόρων είναι μια συλλογή από τους πόρους που κάνουν κοινή χρήση κύκλου ζωής, δικαιώματα και πολιτικές. Μάθετε περισσότερα [εδώ](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Στην **τοποθεσία**, επιλέξτε τη γεωγραφική περιοχή θα χρησιμοποιείται για να αποθηκεύσετε τις εγγραφές μέσα και μετα-δεδομένων για το λογαριασμό σας Media Services. Αυτήν την περιοχή χρησιμοποιείται για την επεξεργασία και ροή πολυμέσων σας. Μόνο τις διαθέσιμες υπηρεσίες πολυμέσων περιοχές που εμφανίζονται στο πλαίσιο αναπτυσσόμενης λίστας. 
    
    3. Στο **Χώρο αποθήκευσης λογαριασμού**, επιλέξτε ένα λογαριασμό χώρου αποθήκευσης για την παροχή χώρος αποθήκευσης αντικειμένων blob του περιεχομένου πολυμέσων από το λογαριασμό σας Media Services. Μπορείτε να επιλέξετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης στην ίδια γεωγραφική περιοχή με το λογαριασμό σας Media Services ή μπορείτε να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης. Δημιουργείται ένα νέο λογαριασμό του χώρου αποθήκευσης στην ίδια περιοχή. Οι κανόνες για τα ονόματα λογαριασμού χώρου αποθήκευσης είναι η ίδια όπως για λογαριασμούς Media Services.

        Μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης [εδώ](storage-introduction.md).

    4. Επιλέξτε **το Pin στον πίνακα εργαλείων** για να δείτε την πρόοδο της ανάπτυξης λογαριασμού.
    
7. Κάντε κλικ στην επιλογή **Δημιουργία** στο κάτω μέρος της φόρμας.

    Αφού δημιουργηθεί το λογαριασμό με επιτυχία, η κατάσταση αλλάζει για να **εκτελείται**. 

    ![Ρυθμίσεις των υπηρεσιών πολυμέσων](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Για να διαχειριστείτε το λογαριασμό σας αθροιστικής μέτρησης ενισχύσεων (για παράδειγμα, αποστολή βίντεο, κωδικοποιείτε περιουσιακών στοιχείων, παρακολούθηση της προόδου εργασίας) Χρησιμοποιήστε το παράθυρο διαλόγου **Ρυθμίσεις** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Ρύθμιση παραμέτρων ροής τελικά σημεία με την πύλη Azure

Όταν εργάζεστε με υπηρεσιών Azure Media Services ένα από τα πιο συνηθισμένα σενάρια την εκτέλεση του βίντεο μέσω προσαρμόσιμη ρυθμό μετάδοσης bit ροής για τους πελάτες σας. Υπηρεσίες πολυμέσων υποστηρίζει το παρακάτω προσαρμόσιμη ρυθμό με μετάδοσης bit ροής τεχνολογίες: HTTP Live ροής (HLS), ομαλή ροή, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ (για αδειών Adobe PrimeTime/πρόσβαση μόνο).

Υπηρεσίες πολυμέσων παρέχει δυναμική συσκευασίας, η οποία σας επιτρέπει να κάνουν σας προσαρμόσιμες ρυθμό μετάδοσης bit MP4 κωδικοποιημένη περιεχομένου σε ροή μορφές που υποστηρίζονται από τις υπηρεσίες πολυμέσων (MPEG ΠΑΎΛΑ, HLS, ομαλή ροή, σκληροί ΔΊΣΚΟΙ) μόνο στιγμή, χωρίς να χρειάζεται να αποθηκεύετε προ-συσκευασμένη εκδόσεις του κάθε μία από αυτές τις μορφές ροής.

Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να κάνετε τα εξής:

- Κωδικοποίηση του αρχείου θυγατρική (προέλευση) σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχείων (τα βήματα κωδικοποίησης εκδηλώνονται παρακάτω σε αυτό το πρόγραμμα εκμάθησης).  
- Δημιουργία τουλάχιστον μία ροή μονάδα για τη *ροή τελικό σημείο* από το οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Τα παρακάτω βήματα δείχνουν πώς μπορείτε να αλλάξετε τον αριθμό των μονάδων ροής.

Με δυναμική συσκευασία, μόνο πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και Media Services δημιουργεί και εξυπηρετεί την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη.

Για να δημιουργήσετε και να αλλάξετε τον αριθμό των ροής δεσμευμένη μονάδες, κάντε τα εξής:


1. Στο παράθυρο " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **ροής τελικά σημεία**. 

2. Κάντε κλικ στην προεπιλεγμένη ροή τελικού σημείου. 

    Εμφανίζεται το παράθυρο **ΠΡΟΕΠΙΛΕΓΜΈΝΗ ΡΟΉ ΛΕΠΤΟΜΈΡΕΙΕΣ τελικού ΣΗΜΕΊΟΥ** .

3. Για να καθορίσετε τον αριθμό των μονάδων ροής, σύρετε το ρυθμιστικό **μονάδες ροής** .

    ![Η ροή μονάδες](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τις αλλαγές σας.

    >[AZURE.NOTE]Η εκχώρηση οποιαδήποτε νέα μονάδων μπορεί να διαρκέσει έως και 20 λεπτά για να ολοκληρωθεί.

##<a name="create-and-configure-a-visual-studio-project"></a>Δημιουργία και ρύθμιση παραμέτρων ένα έργο Visual Studio

1. Δημιουργία νέας εφαρμογής κονσόλας C# στο Visual Studio 2013, το Visual Studio 2012 ή Visual Studio 2010 SP1. Πληκτρολογήστε το **όνομα**, **θέση**και **όνομα λύσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

2. Χρησιμοποιήστε το πακέτο NuGet [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) για να εγκαταστήσετε το **Azure Media Services .NET SDK επεκτάσεις**.  Οι επεκτάσεις Media Services .NET SDK είναι ένα σύνολο μεθόδων επέκταση και συναρτήσεις Βοήθειας που θα απλοποιήσετε τον κώδικά σας και να σας διευκολύνουν να αναπτύξω με τις υπηρεσίες πολυμέσων. Κατά την εγκατάσταση αυτού του πακέτου, εγκαθιστά **Media Services.NET SDK** και επίσης προσθέτει όλες τις άλλες απαιτούμενες εξαρτήσεις.

3. Προσθήκη αναφοράς σε System.Configuration συγκρότησης. Αυτής της συγκρότησης περιέχει την κλάση **System.Configuration.ConfigurationManager** που χρησιμοποιείται για να αποκτήσετε πρόσβαση σε αρχεία ρύθμισης παραμέτρων, για παράδειγμα, App.config.

4. Ανοίξτε το αρχείο App.config (προσθέστε το αρχείο στο έργο σας, εάν δεν έχει προστεθεί από προεπιλογή) και να προσθέσετε μια ενότητα *appSettings* το αρχείο. Ορίστε τις τιμές για υπηρεσιών Azure Media Services και του ονόματος λογαριασμού το κλειδί λογαριασμού σας, όπως φαίνεται στο παρακάτω παράδειγμα. Για να αποκτήσετε το όνομα λογαριασμού και βασικές πληροφορίες, μεταβείτε στην [πύλη του Azure](https://portal.azure.com/) και επιλέξτε το λογαριασμό σας αθροιστικής μέτρησης ενισχύσεων. Στη συνέχεια, επιλέξτε **Ρυθμίσεις** > **πλήκτρα**. Η διαχείριση windows πλήκτρα εμφανίζει το όνομα του λογαριασμού και τα πλήκτρα κύριας και δευτερεύουσας εμφανίζεται.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Για να αντικαταστήσετε τις υπάρχουσες **χρησιμοποιώντας** προτάσεις στην αρχή του αρχείου Program.cs με τον ακόλουθο κώδικα.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Δημιουργία νέου φακέλου κάτω από τον κατάλογο έργα και αντιγραφή αρχείου .mp4 ή .wmv που θέλετε να κωδικοποιήσετε και ροή ή σταδιακά λήψη. Σε αυτό το παράδειγμα, χρησιμοποιείται η διαδρομή "C:\VideoFiles".

##<a name="connect-to-the-media-services-account"></a>Σύνδεση με το λογαριασμό Media Services

Όταν χρησιμοποιείτε τις υπηρεσίες πολυμέσων με .NET, πρέπει να χρησιμοποιήσετε την κλάση **CloudMediaContext** για περισσότερες υπηρεσίες πολυμέσων του προγραμματισμού εργασιών: σύνδεση στο λογαριασμό Media Services; Δημιουργία, ενημέρωση, την πρόσβαση και τη διαγραφή τα ακόλουθα αντικείμενα: περιουσιακών στοιχείων, αρχεία περιουσιακών στοιχείων, εργασίες, οι πολιτικές πρόσβασης, locators, κ.λπ.

Για να αντικαταστήσετε την προεπιλεγμένη κλάση πρόγραμμα με τον ακόλουθο κώδικα. Ο κώδικας περιγράφει τον τρόπο ανάγνωσης των τιμών της σύνδεσης από το αρχείο App.config και πώς μπορείτε να δημιουργήσετε το αντικείμενο **CloudMediaContext** προκειμένου να συνδεθεί με τις υπηρεσίες πολυμέσων. Για περισσότερες πληροφορίες σχετικά με τη σύνδεση με τις υπηρεσίες πολυμέσων, ανατρέξτε στο θέμα [σύνδεση με τις υπηρεσίες πολυμέσων με το SDK υπηρεσίες πολυμέσων για .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Η συνάρτηση **κύριες** κλήσεις μεθόδους που θα οριστούν περαιτέρω σε αυτήν την ενότητα.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Δημιουργήστε ένα νέο πάγιο και στείλτε ένα αρχείο βίντεο

Υπηρεσίες πολυμέσων που Αποστολή (ή ingest) ψηφιακή τα αρχεία σας στην ενός περιουσιακού στοιχείου. Η οντότητα **περιουσιακών στοιχείων** μπορεί να περιέχει βίντεο, ήχο, εικόνες, μικρογραφιών συλλογές, κείμενο κομμάτια και κλειστές λεζάντες αρχείων (και τα μετα-δεδομένα σχετικά με αυτά τα αρχεία.)  Όταν τα αρχεία έχουν αποσταλεί, το περιεχόμενό σας αποθηκεύονται με ασφάλεια στο cloud για περαιτέρω επεξεργασία και ροής. Τα αρχεία του παγίου ονομάζονται **Αρχεία περιουσιακών στοιχείων**.

Η μέθοδος **UploadFile θα** που ορίζονται από το κάτω από τις κλήσεις **CreateFromFile** (ορίζονται στο .NET επεκτάσεις SDK). **CreateFromFile** δημιουργεί ένα νέο πάγιο στο οποίο βρίσκεται η αποστολή του αρχείου καθορισμένη προέλευση.

Η μέθοδος **CreateFromFile** λαμβάνει **AssetCreationOptions** , η οποία σας επιτρέπει να καθορίσετε μία από τις παρακάτω επιλογές δημιουργίας περιουσιακών στοιχείων:

- **Κανένα** - χωρίς κρυπτογράφηση χρησιμοποιείται. Αυτή είναι η προεπιλεγμένη τιμή. Σημειώστε ότι όταν χρησιμοποιείτε αυτήν την επιλογή, το περιεχόμενό σας δεν είναι προστατευμένο κατά τη μεταφορά ή στο υπόλοιπο στο χώρο αποθήκευσης.
Εάν σκοπεύετε να κάνετε μια MP4 χρησιμοποιώντας προοδευτική λήψη, χρησιμοποιήστε αυτήν την επιλογή.
- **StorageEncrypted** - Χρησιμοποιήστε αυτήν την επιλογή για την κρυπτογράφηση των περιεχόμενό σας Απαλοιφή τοπικά χρησιμοποιώντας κρυπτογράφηση-256 bit για προχωρημένους πρότυπο κρυπτογράφησης (AES), η οποία αποστέλλει, στη συνέχεια, όπου είναι αποθηκευμένο χώρο αποθήκευσης Azure κρυπτογραφημένα στο υπόλοιπο. Στοιχεία που προστατεύονται με κρυπτογράφηση χώρου αποθήκευσης είναι αυτόματα χωρίς κρυπτογράφηση τοποθετείται σε ένα σύστημα κρυπτογραφημένο αρχείο πριν από την κωδικοποίηση και προαιρετικά κρυπτογραφημένα εκ νέου πριν από την αποστολή ξανά ως νέο πάγιο εξόδου. Βασική χρήση συμβαίνει κρυπτογράφησης χώρου αποθήκευσης είναι όταν θέλετε να προστατεύσετε τα αρχεία πολυμέσων εισαγωγής υψηλής ποιότητας με ισχυρή κρυπτογράφηση στο υπόλοιπο στο δίσκο.
- **CommonEncryptionProtected** - Χρησιμοποιήστε αυτήν την επιλογή εάν στέλνετε περιεχομένου που έχει ήδη κρυπτογραφημένα και προστατεύεται με κοινές κρυπτογράφηση ή PlayReady DRM (για παράδειγμα, ομαλή ροή προστατεύεται με PlayReady DRM).
- **EnvelopeEncryptionProtected** -Χρησιμοποιήστε αυτήν την επιλογή εάν στέλνετε HLS κρυπτογραφημένα με AES. Σημειώστε ότι τα αρχεία πρέπει να έχουν κωδικοποιημένη και κρυπτογραφημένα με μετασχηματισμός Manager.

Η μέθοδος **CreateFromFile** σάς επιτρέπει επίσης να καθορίσετε μια επιστροφή κλήσης για να αναφέρετε την πρόοδο αποστολή του αρχείου.

Στο παρακάτω παράδειγμα, θα σας καθορίστε **κανένα** για τις επιλογές περιουσιακών στοιχείων.

Προσθέστε την ακόλουθη μέθοδο για την τάξη πρόγραμμα.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Κωδικοποίηση του αρχείου προέλευσης σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχείων

Μετά την ingesting περιουσιακών στοιχείων σε Media Services, μπορεί να είναι πολυμέσων κωδικοποιημένη, transmuxed, Υδατογραφημένο, και ούτω καθεξής, προτού παραδοθεί σε υπολογιστές-πελάτες. Αυτές οι δραστηριότητες είναι προγραμματισμένη και να εκτελεστεί σε πολλές παρουσίες ρόλο φόντου για να βεβαιωθείτε ότι υψηλές επιδόσεις και τη διαθεσιμότητα. Αυτές οι δραστηριότητες ονομάζονται εργασίες και κάθε εργασία είναι αποτελούνται από μεμονωμένες εργασίες που κάνετε την πραγματική εργασία στο αρχείο περιουσιακών στοιχείων.

Όπως αναφέρθηκε προηγουμένως, όταν εργάζεστε με υπηρεσιών Azure Media Services, ένα από τα πιο συνηθισμένα σενάρια προσφέρει προσαρμόσιμη ρυθμό μετάδοσης bit ροής για τους πελάτες σας. Υπηρεσίες πολυμέσων δυναμικά να συσκευάσετε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχείων σε μια από τις παρακάτω μορφές: HTTP Live ροής (HLS), ομαλή ροή, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ (για αδειών Adobe PrimeTime/πρόσβαση μόνο).

Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να κάνετε τα εξής:

- Μετατρέπει το θυγατρική (προέλευση) αρχείου σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 ή προσαρμόσιμη ρυθμό μετάδοσης bit ομαλή ροή αρχείων ή κωδικοποίηση.  
- Λήψη τουλάχιστον μία ροή μονάδας για τη ροή τελικό σημείο από τον οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας.

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να υποβάλετε μια εργασία κωδικοποίησης. Η εργασία περιέχει μία εργασία που καθορίζει μετατρέπει το αρχείο θυγατρική σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4s χρησιμοποιώντας **Media Encoder τυπική**. Ο κώδικας υποβάλλει την εργασία και χρειάζεται να περιμένει μέχρι να ολοκληρωθεί.

Μόλις ολοκληρωθεί η εργασία, θα μπορέσετε να ροή σας περιουσιακών στοιχείων ή σταδιακά λήψη αρχείων MP4 που δημιουργήθηκαν ως αποτέλεσμα της μετατροπής.
Σημειώστε ότι δεν χρειάζεται να έχετε περισσότερες από 0 ροής μονάδες προκειμένου να σταδιακά λήψη αρχείων MP4.

Προσθέστε την ακόλουθη μέθοδο για την τάξη πρόγραμμα.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Δημοσίευση του περιουσιακού στοιχείου και να λάβετε διευθύνσεις URL για λήψη ροής και προοδευτική

Ροή ή λήψη ενός περιουσιακού στοιχείου, πρέπει πρώτα να "Δημοσίευση", δημιουργώντας ένα προσδιοριστικό. Locators παρέχουν πρόσβαση σε αρχεία που περιέχονται στο περιουσιακού στοιχείου. Υπηρεσίες πολυμέσων υποστηρίζει δύο τύπους locators: locators OnDemandOrigin, χρησιμοποιούνται για ροή πολυμέσων (για παράδειγμα, MPEG ΠΑΎΛΑΣ, HLS ή ομαλή ροή) και locators υπογραφή (συσχετισμών Ασφαλείας πρόσβασης), που χρησιμοποιείται για τη λήψη αρχείων πολυμέσων.

Αφού δημιουργήσετε το locators, μπορείτε να δημιουργήσετε τις διευθύνσεις URL που χρησιμοποιούνται για τη ροή ή λήψη των αρχείων σας.


Ροή διεύθυνση URL για την ομαλή ροή έχει την παρακάτω μορφή:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Ροή διεύθυνση URL για την HLS έχει την παρακάτω μορφή:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Ροή διεύθυνση URL για την ΠΑΎΛΑ MPEG έχει την παρακάτω μορφή:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Μια διεύθυνση URL συσχετισμών Ασφαλείας που χρησιμοποιείται για να κάνετε λήψη αρχείων έχει την παρακάτω μορφή:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Επεκτάσεις Media Services .NET SDK παρέχουν εύκολη Βοήθειας μεθόδους που επιστρέφουν μορφοποιημένο διευθύνσεις URL για τη δημοσιευμένη περιουσιακών στοιχείων.

Ο ακόλουθος κώδικας χρησιμοποιεί .NET SDK επεκτάσεις για να δημιουργήσετε locators και να λάβετε ροής και προοδευτική λήψη διευθύνσεις URL. Ο κώδικας εμφανίζει επίσης πώς μπορείτε να κάνετε λήψη αρχείων σε έναν τοπικό φάκελο.

Προσθέστε την ακόλουθη μέθοδο για την τάξη πρόγραμμα.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Δοκιμή με την αναπαραγωγή του περιεχομένου  

Όταν εκτελείτε το πρόγραμμα που ορίζονται από το στην προηγούμενη ενότητα, τις διευθύνσεις URL που είναι παρόμοιο με το ακόλουθο θα εμφανίζεται στο παράθυρο της κονσόλας.

Προσαρμόσιμες ροής διευθύνσεις URL:

Ομαλή ροή

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG ΠΑΎΛΑ

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Προοδευτική λήψη διευθύνσεις URL (ήχος και βίντεο).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Για τη ροή βίντεο, χρησιμοποιήστε το [Πρόγραμμα αναπαραγωγής υπηρεσίες πολυμέσων Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Για να ελέγξετε προοδευτική λήψη, επικολλήστε μια διεύθυνση URL στο πρόγραμμα περιήγησης (για παράδειγμα, του Internet Explorer, Chrome ή Safari).


##<a name="next-steps-media-services-learning-paths"></a>Επόμενα βήματα: Media Services διαδικασίες εκμάθησης

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Αναζητάτε κάτι άλλο;

Εάν αυτό το θέμα δεν περιέχουν τι περιμένατε, κάτι που λείπει ή σε κάποιο άλλο τρόπο δεν καλύπτει τις ανάγκες σας, πρέπει να δώσετε μας με χρήση του παρακάτω Disqus νήματος τα σχόλιά σας.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
