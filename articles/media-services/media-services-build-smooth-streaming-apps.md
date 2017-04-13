<properties 
    pageTitle="Ομαλά ροής του Windows Store App εκμάθηση | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε υπηρεσιών Azure Media Services για να δημιουργήσετε μια εφαρμογή του Windows Store C# με ένα στοιχείο ελέγχου XML MediaElement ομαλή ροή αναπαραγωγή περιεχομένου." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Πώς να δημιουργείτε ομαλή ροή εφαρμογής του Windows Store

Η ομαλή ροή προγράμματος-πελάτη SDK για Windows 8 επιτρέπει στους προγραμματιστές να δημιουργήσετε εφαρμογές για το Windows Store που μπορεί να αναπαραγάγει στη ζήτηση και ζωντανή ομαλή ροή περιεχομένου. Εκτός από το βασικό αναπαραγωγής ομαλή ροή περιεχομένου, το SDK παρέχει επίσης εμπλουτισμένες δυνατότητες, όπως η Microsoft PlayReady προστασίας, ποιότητα ήχου επίπεδο περιορισμού, Live DVR, ροή μετάβασης, ακρόαση για ενημερώσεις κατάστασης (όπως αλλαγές σε επίπεδο ποιότητας) και τα συμβάντα σφάλματος και ούτω καθεξής. Για περισσότερες πληροφορίες από τις υποστηριζόμενες δυνατότητες, ανατρέξτε στο θέμα το [Σημειώσεις έκδοσης](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πρόγραμμα αναπαραγωγής Framework για Windows 8](http://playerframework.codeplex.com/). 

Αυτό το πρόγραμμα εκμάθησης περιέχει τέσσερις μαθήματα:

1. Δημιουργία βασικού ομαλή ροή εφαρμογής αποθήκευσης
2. Προσθέστε μια γραμμή ρυθμιστικού για να ελέγξετε την πρόοδο πολυμέσων
3. Επιλέξτε ομαλή ροή ροών
4. Επιλέξτε κομμάτια ομαλή ροή

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Windows 8 32-bit ή 64-bit. Μπορείτε να λάβετε [Τα Windows 8 για μεγάλες επιχειρήσεις αξιολόγησης](http://msdn.microsoft.com/evalcenter/jj554510.aspx) από το MSDN.
- Visual Studio 2012 ή Visual Studio Express 2012 (ή νεότερη έκδοση). Μπορείτε να λάβετε τη δοκιμαστική έκδοση από [εδώ](http://www.microsoft.com/visualstudio/11/downloads).
- Η [Microsoft ομαλής ροής προγράμματος-πελάτη SDK για τα Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Η ολοκληρωμένη λύση για κάθε μαθήματος μπορούν να ληφθούν από δείγματα κώδικα για προγραμματιστές MSDN (κώδικας συλλογή): 

- [Μάθημα 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - απλή Windows 8 ομαλή ροή Media Player, 
- Έλεγχος [Μάθημα 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - ένα απλό Windows 8 ομαλή ροή πρόγραμμα αναπαραγωγής πολυμέσων με γραμμή ρυθμιστικού, 
- [Μάθημα 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - Windows 8 ομαλή ροή πρόγραμμα αναπαραγωγής πολυμέσων με επιλογή ροής,  
- [Μάθημα 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Windows 8 ομαλή ροή πρόγραμμα αναπαραγωγής πολυμέσων με επιλογή παρακολούθηση.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Μάθημα 1: Δημιουργία βασικού ομαλή ροή εφαρμογής αποθήκευσης

Σε αυτό το μάθημα, θα δημιουργήσετε μια εφαρμογή του Windows Store με ένα στοιχείο ελέγχου MediaElement για την αναπαραγωγή ομαλή ροή περιεχομένου.  Η εφαρμογή που εκτελείται έχει την παρακάτω:

![Παράδειγμα εφαρμογής ομαλής ροής Windows Store][PlayerApplication]
 
Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη εφαρμογής Windows Store, ανατρέξτε στο θέμα [Ανάπτυξη εξαιρετική εφαρμογές για το Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Αυτό το μάθημα περιέχει τις παρακάτω διαδικασίες:

1.  Δημιουργία έργου του Windows Store
2.  Σχεδίαση του περιβάλλοντος εργασίας χρήστη (XAML)
3.  Τροποποιήστε τον κώδικα πίσω από αρχείο
4.  Μεταγλώττιση και να ελέγξετε την εφαρμογή

**Για να δημιουργήσετε ένα έργο του Windows Store**

1.  Εκτελέστε το Visual Studio 2012 ή νεότερη έκδοση.
2.  Από το μενού **ΑΡΧΕΊΟ** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.
3.  Από το παράθυρο διαλόγου νέο έργο, πληκτρολογήστε ή επιλέξτε τις ακόλουθες τιμές:

Όνομα|Τιμή
---|---
Ομάδα των προτύπων|Εγκατεστημένη/πρότυπα/Visual C# / χώρου αποθήκευσης των Windows
Πρότυπο|Κενή εφαρμογή (XAML)
Όνομα|SSPlayer
Θέση|C:\SSTutorials
Όνομα της λύσης|SSPlayer
Δημιουργία καταλόγου για τη λύση|(επιλεγμένο)

4.  Κάντε κλικ στο **κουμπί OK**.

**Για να προσθέσετε μια αναφορά στο ομαλή SDK ροής προγράμματος-πελάτη**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ **SSPlayer**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη αναφοράς**.
2.  Πληκτρολογήστε ή επιλέξτε τις ακόλουθες τιμές:

Όνομα|Τιμή
---|---
Ομάδα αναφοράς|Windows/επεκτάσεις
Αναφορά|Επιλέξτε Microsoft ομαλής ροής SDK προγράμματος-πελάτη για τα Windows 8 και Microsoft Visual C++ Runtime πακέτου
    
3.  Κάντε κλικ στο **κουμπί OK**. 

Αφού προσθέσετε τις αναφορές, πρέπει να επιλέξετε την πλατφόρμα προορισμού (x64 ή x86), δεν θα λειτουργεί προσθέτοντας αναφορές για ρύθμιση παραμέτρων της πλατφόρμας οποιαδήποτε CPU.  Στην Εξερεύνηση λύσεων, θα δείτε κίτρινο σημάδι προειδοποίηση για αυτές τις προστεθεί αναφορές.

**Για να σχεδιάσετε το περιβάλλον εργασίας χρήστη του προγράμματος αναπαραγωγής**

1.  Από την Εξερεύνηση λύσεων, κάντε διπλό κλικ **MainPage.xaml** για να την ανοίξετε σε προβολή σχεδίασης.
2.  Εντοπίστε το ** &lt;πλέγμα&gt; ** και ** &lt;/Grid&gt; ** το αρχείο XAML των ετικετών και επικολλήστε τον ακόλουθο κώδικα ανάμεσα στις δύο ετικέτες:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Το στοιχείο ελέγχου MediaElement χρησιμοποιείται για την αναπαραγωγή πολυμέσων. Το στοιχείο ελέγχου ρυθμιστικό με το όνομα sliderProgress θα χρησιμοποιηθεί στο επόμενο μάθημα για να ελέγξετε την πρόοδο πολυμέσων.

3.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

Το στοιχείο ελέγχου MediaElement δεν υποστηρίζει ομαλή ροή περιεχομένου της πρώτης. Για να ενεργοποιήσετε την υποστήριξη ομαλή ροή, πρέπει να καταχωρήσετε την ομαλή ροή πρόγραμμα χειρισμού ροή byte με επέκταση ονόματος αρχείου και τύπος MIME.  Για να καταχωρήσετε, μπορείτε να χρησιμοποιήσετε τη μέθοδο MediaExtensionManager.RegisterByteStremHandler του χώρου ονομάτων Windows.Media.

Σε αυτό το αρχείο XAML, ορισμένα προγράμματα χειρισμού συμβάντων σχετίζονται με τα στοιχεία ελέγχου.  Πρέπει να ορίσετε αυτά τα προγράμματα χειρισμού συμβάντων.

**Για να τροποποιήσετε τον κώδικα πίσω από αρχείο**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2.  Στο επάνω μέρος του αρχείου, προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using Windows.Media;

3.  Κατά την έναρξη της κλάσης **MainPage** , προσθέστε το παρακάτω μέλος δεδομένων:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Στο τέλος της κατασκευής **MainPage** , προσθέστε τις ακόλουθες δύο γραμμές:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Στο τέλος της κλάσης **MainPage** , πέρα από τον ακόλουθο κώδικα:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Το πρόγραμμα χειρισμού συμβάντων sliderProgress_PointerPressed ορίζεται εδώ.  Υπάρχουν περισσότερες έργα για να κάνετε για να λειτουργήσει, τα οποία θα καλύπτονται στο επόμενο μάθημα αυτού του προγράμματος εκμάθησης.
6.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

Το τελικό τον κώδικα πίσω από το αρχείο θα μοιάζει κάπως έτσι:

![Codeview σε εφαρμογή του Visual Studio της ομαλή ροή του Windows Store][CodeViewPic]

**Για να μεταγλωττίσετε και να ελέγξετε την εφαρμογή**

1.  Από το μενού " **ΔΗΜΙΟΥΡΓΊΑ** ", κάντε κλικ στην επιλογή **Διαχείριση ομάδας παραμέτρων**.
2.  Αλλαγή **ενεργή λύση πλατφόρμα** ώστε να ταιριάζει με την πλατφόρμα ανάπτυξης.
3.  Πατήστε το πλήκτρο **F6** για να μεταγλωττίσετε το έργο. 
4.  Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.
5.  Στο επάνω μέρος της εφαρμογής, μπορείτε να χρησιμοποιήσετε το προεπιλεγμένο ομαλή ροή διεύθυνση URL ή να εισαγάγετε ένα διαφορετικό. 
6.  Κάντε κλικ στην επιλογή **Ορισμός προέλευσης**. Επειδή η **Αυτόματη αναπαραγωγή** είναι ενεργοποιημένη από προεπιλογή, τα πολυμέσα μέλη αυτόματη αναπαραγωγή.  Μπορείτε να ελέγχετε τα πολυμέσα που χρησιμοποιεί την **αναπαραγωγή**, **Παύση** "και" **Διακοπή** κουμπιά.  Μπορείτε να ελέγξετε την ένταση ήχου πολυμέσων χρησιμοποιώντας το ρυθμιστικό "Κατακόρυφος".  Ωστόσο το οριζόντιο ρυθμιστικό για τον έλεγχο της προόδου πολυμέσων δεν έχει εφαρμοστεί πλήρως ακόμα. 

Ολοκληρώσατε lesson1.  Σε αυτό το μάθημα, χρησιμοποιείτε ένα στοιχείο ελέγχου MediaElement για αναπαραγωγή ομαλή ροή περιεχομένου.  Στο επόμενο μάθημα, θα μπορείτε να προσθέσετε ένα ρυθμιστικό για να ελέγξετε την πρόοδο του ομαλή ροή περιεχομένου.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Μάθημα 2: Προσθήκη γραμμή ρυθμιστικού για να ελέγξετε την πρόοδο πολυμέσων
Στο Μάθημα 1, ίσως να δημιουργήσει μια εφαρμογή του Windows Store με ένα στοιχείο ελέγχου MediaElement XAML για αναπαραγωγή περιεχομένου πολυμέσων ομαλή ροή.  Θα ορισμένες από τις συναρτήσεις βασικές πολυμέσων όπως Έναρξη και διακοπή παύση.  Σε αυτό το μάθημα, θα μπορείτε να προσθέσετε ένα στοιχείο ελέγχου γραμμή ρυθμιστικού στην εφαρμογή.

Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε χρονόμετρο για να ενημερώσετε τη θέση ρυθμιστικό με βάση την τρέχουσα θέση του στοιχείου ελέγχου MediaElement.  Το ρυθμιστικό έναρξης και λήξης χρόνου επίσης πρέπει να ενημερωθεί σε περίπτωση ζωντανό περιεχόμενο.  Καλύτερη υπάρχουν στο συμβάν ενημέρωσης προσαρμόσιμη προέλευσης.

Οι προελεύσεις πολυμέσων είναι αντικείμενα που δημιουργούν δεδομένα πολυμέσων.  Η επίλυση προέλευσης λαμβάνει μια διεύθυνση URL ή byte ροή και δημιουργεί το κατάλληλο μέσο προέλευσης για αυτό το περιεχόμενο.  Η επίλυση προέλευσης είναι ο τυπικός τρόπος για τις εφαρμογές για να δημιουργήσετε προελεύσεις πολυμέσων. 

Αυτό το μάθημα περιέχει τις παρακάτω διαδικασίες:

1.  Καταχώρηση το πρόγραμμα χειρισμού ομαλή ροή 
2.  Προσθέστε τα προγράμματα χειρισμού συμβάντων επιπέδου manager προσαρμόσιμη προέλευσης
3.  Προσθέστε τα προγράμματα χειρισμού συμβάντων επιπέδου προσαρμόσιμη προέλευσης
4.  Προσθήκη MediaElement προγράμματα χειρισμού συμβάντων
5.  Προσθήκη ρυθμιστικό αντίστοιχο κώδικα στη γραμμή
6.  Μεταγλώττιση και να ελέγξετε την εφαρμογή

**Για να καταχωρήσετε τη λαβή ροή byte ομαλή ροή και μεταβιβάζουν το propertyset**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ στην επιλογή **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2.  Στην αρχή του αρχείου, προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using Microsoft.Media.AdaptiveStreaming;

3.  Κατά την έναρξη της κλάσης MainPage, προσθέστε τα ακόλουθα μέλη δεδομένων:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Μέσα στην κατασκευή **MainPage** , προσθέστε τον παρακάτω κώδικα μετά το **αυτό. Προετοιμασία Components();** γραμμή και οι γραμμές κώδικα εγγραφής στο προηγούμενο μάθημα:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Μέσα στην κατασκευή **MainPage** , τροποποιήστε τις δύο μεθόδους RegisterByteStreamHandler για να προσθέσετε το τέταρτο παράμετροι:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

**Για να προσθέσετε το πρόγραμμα χειρισμού συμβάντων επιπέδου manager προσαρμόσιμη προέλευσης**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ στην επιλογή **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2.  Μέσα στην κλάση **MainPage** , προσθέστε το παρακάτω μέλος δεδομένων:

        private AdaptiveSource adaptiveSource = null;

3.  Στο τέλος της κλάσης **MainPage** , προσθέστε το παρακάτω πρόγραμμα χειρισμού συμβάντων:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Στο τέλος της κατασκευής **MainPage** , προσθέστε την ακόλουθη γραμμή για να εγγραφείτε για το συμβάν open προσαρμόσιμη προέλευση:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += νέα AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

**Για να προσθέσετε προγράμματα χειρισμού συμβάντων επιπέδου προσαρμόσιμη προέλευσης**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ στην επιλογή **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2.  Μέσα στην κλάση **MainPage** , προσθέστε το παρακάτω μέλος δεδομένων:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Στο τέλος της κλάσης **MainPage** , προσθέστε τα ακόλουθα προγράμματα χειρισμού συμβάντων:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Στο τέλος της μεθόδου **mediaElement AdaptiveSourceOpened** , προσθέστε τον παρακάτω κώδικα για να εγγραφείτε για τα συμβάντα:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

Τα ίδια συμβάντα είναι διαθέσιμα στην προέλευση προσαρμόσιμη διαχείρισης επιπέδου καθώς και, που μπορούν να χρησιμοποιηθούν για το χειρισμό λειτουργίες κοινές σε όλα τα στοιχεία πολυμέσων στην εφαρμογή. Κάθε AdaptiveSource περιλαμβάνει τα δικά της συμβάντα και όλα τα συμβάντα AdaptiveSource θα είναι διαδοχικές στην περιοχή AdaptiveSourceManager.

**Για να προσθέσετε προγράμματα χειρισμού συμβάντων στοιχείο πολυμέσων**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ στην επιλογή **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2.  Στο τέλος της κλάσης **MainPage** , προσθέστε τα ακόλουθα προγράμματα χειρισμού συμβάντων:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Στο τέλος της κατασκευής **MainPage** , προσθέστε τον ακόλουθο κώδικα στο δείκτη στα συμβάντα:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

**Για να προσθέσετε γραμμή ρυθμιστικού που σχετίζονται με κώδικα**

1.  Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ στην επιλογή **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2.  Στην αρχή του αρχείου, προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:
    
        using Windows.UI.Core;

3.  Μέσα στην κλάση **MainPage** , προσθέστε τα ακόλουθα μέλη δεδομένων:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Στο τέλος της κατασκευής **MainPage** , προσθέστε τον ακόλουθο κώδικα:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Στο τέλος της κλάσης **MainPage** , προσθέστε τον ακόλουθο κώδικα:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Σημείωση:** CoreDispatcher χρησιμοποιείται για να κάνετε αλλαγές στο περιβάλλον εργασίας Χρήστη νήμα από μη νήμα περιβάλλοντος εργασίας Χρήστη. Σε περίπτωση συμφόρηση σε νήμα απόσπασης, προγραμματιστής μπορεί να επιλέξετε να χρησιμοποιήσετε απόσπασης που παρέχονται από το στοιχείο περιβάλλοντος εργασίας Χρήστη διενέργεια σκοπεύει να ενημερώσετε.  Για παράδειγμα:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Στο τέλος της μεθόδου **mediaElement_AdaptiveSourceStatusUpdated** , προσθέστε τον ακόλουθο κώδικα:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Στο τέλος της μεθόδου **MediaOpened** , προσθέστε τον ακόλουθο κώδικα:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε το αρχείο.

**Για να μεταγλωττίσετε και να ελέγξετε την εφαρμογή**

1. Πατήστε το πλήκτρο **F6** για να μεταγλωττίσετε το έργο. 
2.  Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.
3.  Στο επάνω μέρος της εφαρμογής, μπορείτε να χρησιμοποιήσετε το προεπιλεγμένο ομαλή ροή διεύθυνση URL ή να εισαγάγετε ένα διαφορετικό. 
4.  Κάντε κλικ στην επιλογή **Ορισμός προέλευσης**. 
5.  Ελέγξτε τη γραμμή ρυθμιστικού.

Ολοκληρώσατε Μάθημα 2.  Σε αυτό το μάθημα προσθέσατε ρυθμιστικού στην εφαρμογή. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Μάθημα 3: Επιλέξτε ομαλή ροή ροών
Ομαλή ροής έχει δυνατότητα για ροή περιεχομένου με πολλά κομμάτια ήχου γλώσσας που είναι επιλέξιμο από τα προγράμματα προβολής.  Σε αυτό το μάθημα, θα μπορείτε να ενεργοποιήσετε προγράμματα προβολής για να επιλέξετε ροών. Αυτό το μάθημα περιέχει τις παρακάτω διαδικασίες:

1. Τροποποιήστε το αρχείο XAML
2. Τροποποιήστε το αρχείο behand κώδικα
3. Μεταγλώττιση και να ελέγξετε την εφαρμογή


**Για να τροποποιήσετε το αρχείο XAML**

1. Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή σχεδίασης**.
2. Εντοπίστε &lt;Grid.RowDefinitions&gt;, και να τροποποιήσετε το RowDefinitions, ώστε να τους έχει την παρακάτω:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Εντός του &lt;πλέγμα&gt;&lt;/Grid&gt; ετικετών, προσθέστε τον ακόλουθο κώδικα για να ορίσετε ένα στοιχείο ελέγχου πλαισίου λίστας, ώστε οι χρήστες μπορούν να δείτε τη λίστα διαθέσιμες ροές και επιλέξτε ροές:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε τις αλλαγές.


**Για να τροποποιήσετε τον κώδικα πίσω από αρχείο**

1. Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2. Μέσα στο χώρο ονομάτων SSPlayer, προσθέστε μια νέα κλάση: #region κλάσης ροής
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Κατά την έναρξη της κλάσης MainPage, προσθέστε τους ακόλουθους ορισμούς μεταβλητής:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Μέσα στην κλάση MainPage, προσθήκη στην παρακάτω περιοχή:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Εντοπίστε τη μέθοδο mediaElement_ManifestReady, προσαρτήσετε τον ακόλουθο κώδικα στο τέλος της συνάρτησης:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Επομένως, όταν MediaElement δηλωτικό είναι έτοιμη, τον κωδικό λαμβάνει μια λίστα με τις διαθέσιμες ροές και συμπληρώνει το πλαίσιο λίστας περιβάλλοντος εργασίας Χρήστη με τη λίστα.

6. Μέσα στην κλάση MainPage, εντοπίστε το περιβάλλον εργασίας Χρήστη κουμπιά κάντε κλικ στην περιοχή συμβάντα και, στη συνέχεια, προσθέστε τον ακόλουθο ορισμό συνάρτηση:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Για να μεταγλωττίσετε και να ελέγξετε την εφαρμογή**

1. Πατήστε το πλήκτρο **F6** για να μεταγλωττίσετε το έργο. 
2.  Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.
3.  Στο επάνω μέρος της εφαρμογής, μπορείτε να χρησιμοποιήσετε το προεπιλεγμένο ομαλή ροή διεύθυνση URL ή να εισαγάγετε ένα διαφορετικό. 
4.  Κάντε κλικ στην επιλογή **Ορισμός προέλευσης**. 
5.  Η προεπιλεγμένη γλώσσα είναι audio_eng. Δοκιμάστε να κάνετε εναλλαγή μεταξύ audio_eng και audio_es. Κάθε φορά, επιλέξτε μια νέα ροή, πρέπει να κάνετε κλικ στο κουμπί υποβολή.

Ολοκληρώσατε Μάθημα 3.  Σε αυτό το μάθημα, μπορείτε να προσθέσετε τη λειτουργικότητα για να επιλέξετε ροών.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Μάθημα 4: Επιλέξτε κομμάτια ομαλή ροή
Ομαλή ροή παρουσίασης μπορούν να περιέχουν πολλά αρχεία βίντεο κωδικοποιηθεί με ποιότητα διαφορετικά επίπεδα (ταχύτητες bit) και λύσεις. Σε αυτό το μάθημα, που θα επιτρέπουν στους χρήστες για να επιλέξετε κομμάτια. Αυτό το μάθημα περιέχει τις παρακάτω διαδικασίες:

1. Τροποποιήστε το αρχείο XAML
2. Τροποποιήστε το αρχείο behand κώδικα
3. Μεταγλώττιση και να ελέγξετε την εφαρμογή

**Για να τροποποιήσετε το αρχείο XAML**

1. Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή σχεδίασης**.
2. Εντοπίστε το &lt;πλέγμα&gt; ετικέτα με το όνομα **gridStreamAndBitrateSelection**, να προσαρτήσετε τον ακόλουθο κώδικα στο τέλος της ετικέτας:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Πατήστε το **Συνδυασμό πλήκτρων CTRL + S** για να αποθηκεύσετε τις αλλαγές he


**Για να τροποποιήσετε τον κώδικα πίσω από αρχείο**

1. Από την Εξερεύνηση λύσεων, κάντε δεξί κλικ **MainPage.xaml**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή κώδικα**.
2. Μέσα στο χώρο ονομάτων SSPlayer, προσθέστε μια νέα κλάση:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Κατά την έναρξη της κλάσης MainPage, προσθέστε τους ακόλουθους ορισμούς μεταβλητής:
    
        private List<Track> availableTracks;

4. Μέσα στην κλάση MainPage, προσθήκη στην παρακάτω περιοχή:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Εντοπίστε τη μέθοδο mediaElement_ManifestReady, προσαρτήσετε τον ακόλουθο κώδικα στο τέλος της συνάρτησης:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Μέσα στην κλάση MainPage, εντοπίστε το περιβάλλον εργασίας Χρήστη κουμπιά κάντε κλικ στην περιοχή συμβάντα και, στη συνέχεια, προσθέστε τον ακόλουθο ορισμό συνάρτηση:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Για να μεταγλωττίσετε και να ελέγξετε την εφαρμογή**

1. Πατήστε το πλήκτρο **F6** για να μεταγλωττίσετε το έργο. 
2.  Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.
3.  Στο επάνω μέρος της εφαρμογής, μπορείτε να χρησιμοποιήσετε το προεπιλεγμένο ομαλή ροή διεύθυνση URL ή να εισαγάγετε ένα διαφορετικό. 
4.  Κάντε κλικ στην επιλογή **Ορισμός προέλευσης**. 
5.  Από προεπιλογή, όλα τα κομμάτια από τη ροή βίντεο είναι επιλεγμένο. Για να πειραματιστείτε τις αλλαγές επιτόκιο bit, μπορείτε να επιλέξετε στην χαμηλότερη διαθέσιμη ταχύτητα bit και, στη συνέχεια, επιλέξτε το μεγαλύτερο ρυθμό bit που είναι διαθέσιμες. Θα πρέπει να κάντε κλικ στην επιλογή υποβολή μετά από κάθε αλλαγή.  Μπορείτε να δείτε τις αλλαγές της ποιότητας του βίντεο.

Ολοκληρώσατε Μάθημα 4.  Σε αυτό το μάθημα, μπορείτε να προσθέσετε τη λειτουργικότητα για να επιλέξετε κομμάτια.


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Άλλοι πόροι:
- [Πώς να δημιουργείτε μια εφαρμογή ομαλή ροή των Windows 8 κώδικα JavaScript με δυνατότητες για προχωρημένους](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Ομαλή ροή Τεχνική επισκόπηση](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
