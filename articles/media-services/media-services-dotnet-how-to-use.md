<properties 
    pageTitle="Πώς μπορείτε να ρυθμίσετε τον υπολογιστή για ανάπτυξη με το .NET υπηρεσίες πολυμέσων" 
    description="Μάθετε περισσότερα σχετικά με τις προϋποθέσεις για τις υπηρεσίες πολυμέσων με χρήση του SDK υπηρεσίες πολυμέσων για .NET. Επίσης, μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή του Visual Studio." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Ανάπτυξη Media Services με .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Αυτό το θέμα περιγράφει πώς να ξεκινήσετε την ανάπτυξη εφαρμογών Media Services χρησιμοποιώντας .NET.

Η βιβλιοθήκη **Azure Media Services.NET SDK** σάς επιτρέπει να πρόγραμμα σε σχέση με υπηρεσίες πολυμέσων χρησιμοποιώντας .NET. Για να κάνετε ακόμα ευκολότερη την ανάπτυξη με το .NET, παρέχεται η βιβλιοθήκη **Azure Media Services .NET SDK επεκτάσεις** . Αυτή η βιβλιοθήκη περιέχει ένα σύνολο μεθόδων επέκταση και συναρτήσεις Βοήθειας που θα απλοποιήσει τον κωδικό .NET. Βιβλιοθήκες και τα δύο είναι διαθέσιμα μέσω **NuGet** και **GitHub**.


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

-   Ένας λογαριασμός Media Services σε μια νέα ή υπάρχουσα συνδρομή Azure. Ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε ένα λογαριασμό υπηρεσίες πολυμέσων](media-services-portal-create-account.md).
-   Λειτουργικά συστήματα: Windows 10, Windows 7, Windows 2008 R2 ή Windows 8.
-   .NET framework διαίρεσης 4,5.
-    Visual Studio 2015, Visual Studio 2013, το Visual Studio 2012 ή Visual Studio 2010 SP1 (Professional, Premium, Ultimate ή Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Δημιουργία και ρύθμιση παραμέτρων ένα έργο Visual Studio

Αυτή η ενότητα σας δείχνει πώς μπορείτε να δημιουργήσετε ένα έργο στο Visual Studio και να ρυθμίσετε για χρήση με ανάπτυξη Media Services.  Σε αυτήν την περίπτωση το έργο είναι μια εφαρμογή κονσόλας Windows C#, αλλά τα ίδια βήματα εγκατάστασης που φαίνεται εδώ εφαρμόζονται σε άλλους τύπους έργων που μπορείτε να δημιουργήσετε για εφαρμογές υπηρεσιών πολυμέσων (για παράδειγμα, μια εφαρμογή φόρμες των Windows ή μιας εφαρμογής ASP.NET Web).

Αυτή η ενότητα δείχνει πώς μπορείτε να χρησιμοποιήσετε **NuGet** για να προσθέσετε Media Services .NET SDK και σε άλλες βιβλιοθήκες εξαρτώμενα.

Εναλλακτικά, μπορείτε να λάβετε τα πιο πρόσφατα bit Media Services .NET SDK από GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) και [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), να δημιουργήσετε τη λύση και να προσθέσετε τις αναφορές στο έργο προγράμματος-πελάτη. Σημειώστε ότι όλες οι απαραίτητες εξαρτήσεις να λάβει και που έχουν εξαχθεί αυτόματα.

1. Δημιουργία νέας εφαρμογής κονσόλας C# στο Visual Studio 2010 SP1 ή νεότερες εκδόσεις και στο. Πληκτρολογήστε το **όνομα**, **θέση**και **όνομα λύσης**και, στη συνέχεια, κάντε κλικ στο κουμπί OK.

2. Δημιουργία της λύσης.

2. Χρησιμοποιήστε **NuGet** για να εγκαταστήσετε και να προσθέσετε **Azure Media Services .NET SDK επεκτάσεις**. Κατά την εγκατάσταση αυτού του πακέτου, εγκαθιστά **Media Services.NET SDK** και επίσης προσθέτει όλες τις άλλες απαιτούμενες εξαρτήσεις.

    Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του NuGet εγκατεστημένο. Για περισσότερες πληροφορίες και οδηγίες εγκατάστασης, ανατρέξτε στο θέμα [NuGet](http://nuget.codeplex.com/).

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο όνομα του έργου και επιλέξτε Διαχείριση NuGet πακέτων...

    Εμφανίζεται το παράθυρο διαλόγου Διαχείριση πακέτων NuGet.

3. Στη συλλογή Online, αναζήτηση επεκτάσεις MediaServices Azure, επιλέξτε Azure Media Services .NET SDK επεκτάσεις και, στη συνέχεια, κάντε κλικ στο κουμπί εγκατάσταση.

    Τροποποίηση του έργου και προστίθενται οι αναφορές σε οι επεκτάσεις Media Services .NET SDK, Media Services .NET SDK και άλλες εξαρτημένες συγκροτήσεις.

4. Για να προβιβάσετε μια πιο καθαρή περιβάλλον ανάπτυξης, εξετάστε το ενδεχόμενο ενεργοποίησης Επαναφορά πακέτου NuGet. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επαναφορά πακέτου NuGet "](http://docs.nuget.org/consume/package-restore).

3. Προσθήκη αναφοράς σε **System.Configuration** συγκρότησης. Αυτό συναρμολόγησης περιέχει το System.Configuration. Κλάση **ConfigurationManager** που χρησιμοποιείται για να αποκτήσετε πρόσβαση σε αρχεία ρύθμισης παραμέτρων (για παράδειγμα, App.config).

    Για να προσθέσετε αναφορές χρησιμοποιώντας το παράθυρο διαλόγου Διαχείριση αναφορών, κάντε δεξί κλικ στο όνομα του έργου στην Εξερεύνηση λύσεων. Στη συνέχεια, επιλέξτε Προσθήκη και αναφορές.

    Εμφανίζεται το παράθυρο διαλόγου Διαχείριση αναφορών.

4. Στην περιοχή συγκροτήσεις .NET framework, βρείτε και επιλέξτε συγκρότησης System.Configuration και πατήστε το κουμπί OK.
5. Ανοίξτε το αρχείο App.config (προσθέστε το αρχείο στο έργο σας, εάν δεν έχει προστεθεί από προεπιλογή) και να προσθέσετε μια ενότητα *appSettings* το αρχείο.     
Ορίστε τις τιμές για υπηρεσιών Azure Media Services και του ονόματος λογαριασμού το κλειδί λογαριασμού σας, όπως φαίνεται στο παρακάτω παράδειγμα.

    Για να βρείτε το όνομα και το κλειδί τιμές, μεταβείτε στην πύλη του Azure και επιλέξτε το λογαριασμό σας. Εμφανίζεται το παράθυρο διαλόγου ρυθμίσεις στα δεξιά. Στο παράθυρο "Ρυθμίσεις", επιλέξτε κλειδιά. Κάνοντας κλικ στο εικονίδιο δίπλα σε κάθε πλαίσιο κειμένου αντιγράφει την τιμή στο Πρόχειρο του συστήματος.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Για να αντικαταστήσετε τις υπάρχουσες **χρησιμοποιώντας** προτάσεις στην αρχή του αρχείου Program.cs με τον ακόλουθο κώδικα.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

Σε αυτό το σημείο, είστε έτοιμοι να ξεκινήσετε την ανάπτυξη μιας εφαρμογής Media Services.    


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
