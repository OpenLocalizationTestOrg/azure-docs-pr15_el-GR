<properties 
    pageTitle="Φίλτρα και δυναμική δηλώσεων | Microsoft Azure" 
    description="Αυτό το θέμα περιγράφει τον τρόπο δημιουργίας φίλτρων, ώστε το πρόγραμμα-πελάτη να τα χρησιμοποιήσετε για συγκεκριμένες ενότητες ροής μιας ροής. Υπηρεσίες πολυμέσων δημιουργεί δυναμικής δηλώσεων για να αρχειοθετήσετε αυτό επιλεκτική ροής." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Φίλτρα και δυναμική δηλώσεων

Ξεκινώντας με την έκδοση 2.11, Media Services σάς επιτρέπει να ορίσετε φίλτρα για τους πόρους σας. Αυτά τα φίλτρα είναι κανόνων στην πλευρά του διακομιστή που θα επιτρέπουν στους πελάτες σας να επιλέξετε να κάνετε πράγματα όπως: αναπαραγωγή μόνο ενός τμήματος του βίντεο (αντί για αναπαραγωγή του βίντεο ολόκληρη), ή να καθορίσετε μόνο ένα υποσύνολο των αποδόσεων ήχου και βίντεο που μπορεί να διαχειριστεί συσκευή του πελάτη σας (αντί για όλες τις αποδόσεις που συσχετίζονται με το περιουσιακών στοιχείων). Αυτό το φιλτράρισμα από τους πόρους σας αρχειοθετείται μέσω s **Δυναμικής δήλωσης**που έχουν δημιουργηθεί μετά από αίτηση του πελάτη σας για τη ροή βίντεο που βασίζονται σε καθορισμένη φίλτρα.

Το θέμα αυτό περιγράφει κοινά σενάρια στα οποία τα φίλτρα θα είναι ιδιαίτερα χρήσιμη για τους πελάτες και συνδέσεις σε θέματα τα οποία δείχνουν τον τρόπο δημιουργίας φίλτρων μέσω προγραμματισμού (προς το παρόν, μπορείτε να δημιουργήσετε φίλτρα με REST API μόνο).

##<a name="overview"></a>Επισκόπηση

Κατά την παράδοση του περιεχομένου σε πελάτες (ροή ζωντανή συμβάντα ή video-on-demand) στόχος σας είναι να παραδώσετε βίντεο υψηλής ποιότητας σε διάφορες συσκευές υπό όρους διαφορετικό δίκτυο. Για να επιτύχετε αυτό στόχου, κάντε τα εξής:

- κωδικοποίηση σας ροή για τη ροή βίντεο πολλούς ρυθμό μετάδοσης bit ([προσαρμόσιμη ρυθμό μετάδοσης bit](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) (αυτό θα αναλάβουν την ποιότητα και δικτύου συνθήκες) και 
- Χρησιμοποιήστε Media Services [Δυναμικής συσκευασία](media-services-dynamic-packaging-overview.md) για δυναμικά εκ νέου πακέτο σας ροή σε διαφορετικά πρωτόκολλα (αυτό θα εκτελέσει η ροή σε διαφορετικές συσκευές). Υπηρεσίες πολυμέσων υποστηρίζει παράδοση το παρακάτω προσαρμόσιμη ρυθμό με μετάδοσης bit ροής τεχνολογίες: HTTP Live ροής (HLS), ομαλή ροή, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ (για αδειών Adobe PrimeTime/πρόσβαση μόνο). 

###<a name="manifest-files"></a>Δήλωσης αρχείων 

Όταν κωδικοποιείτε ενός περιουσιακού στοιχείου για ροή προσαρμόσιμη ρυθμό μετάδοσης bit, δημιουργείται ένα αρχείο **δηλώσεων** (λίστα αναπαραγωγής) (το αρχείο είναι βασίζεται σε κείμενο ή βασίζονται σε XML). Το αρχείο **δηλώσεων** περιλαμβάνει όπως η ροή μετα-δεδομένων: παρακολούθηση τύπου (ήχο, βίντεο ή κείμενο), παρακολούθηση όνομα, ώρα έναρξης και λήξης, ρυθμό μετάδοσης bit (Ιδιότητες), παρακολούθηση γλωσσών, παράθυρο παρουσίασης (κυλιόμενο παράθυρο σταθερή διάρκεια), κωδικοποιητής βίντεο (FourCC). Το καθοδηγεί επίσης το πρόγραμμα αναπαραγωγής για να ανακτήσετε το επόμενο τμήμα, παρέχοντας πληροφορίες σχετικά με την επόμενη δυνατότητα αναπαραγωγής βίντεο τμήματα διαθέσιμες και τη θέση τους. Τμήματα (ή τμήματα) είναι η πραγματική "μπλοκ" ενός βίντεο περιεχομένου.


Ακολουθεί ένα παράδειγμα δήλωσης αρχείου: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Δυναμική δηλώσεων

Υπάρχουν [σενάρια](media-services-dynamic-manifest-overview.md#scenarios) , όταν το πρόγραμμα-πελάτης πρέπει μεγαλύτερη ευελιξία από ό, τι περιγράφεται σε αρχείο δηλώσεων του παγίου προεπιλογή. Για παράδειγμα:

- Συσκευή συγκεκριμένα: παράδοση μόνο για το καθορισμένο αποδόσεις ή\και καθοριστεί κομμάτια γλώσσας που υποστηρίζονται από τη συσκευή που είναι χρησιμοποιείται για την αναπαραγωγή του περιεχομένου ("απόδοσης φιλτράρισμα"). 
- Μειώστε το δηλωτικό για να εμφανίσετε μια δευτερεύουσα clip της μια ζωντανή ("δευτερεύουσα clip φιλτράρισμα συμβάντων").
- Αποκοπή της έναρξης ενός βίντεο ("Περικοπή βίντεο").
- Προσαρμόστε το παράθυρο παρουσίασης (DVR) για να παρέχουν ένα περιορισμένο μήκος του παραθύρου DVR στο πρόγραμμα αναπαραγωγής ("Προσαρμογή παράθυρο παρουσίασης").
 
Για να επιτύχετε αυτή η ευελιξία, Media Services προσφέρει **Δυναμικές δηλώσεις** που βασίζονται σε προκαθορισμένες [φίλτρα](media-services-dynamic-manifest-overview.md#filters).  Όταν ορίζετε τα φίλτρα, τους πελάτες σας μπορούν να τις χρησιμοποιήσουν για τη ροή μια συγκεκριμένη απόδοσης ή δευτερεύουσες αποσπάσματα του βίντεό σας. Μπορούν να καθορίσουν φίλτρα στη ροή διεύθυνση URL. Φίλτρα μπορεί να εφαρμοστούν σε προσαρμόσιμες ρυθμό μετάδοσης bit ροής πρωτόκολλα που υποστηρίζονται από [Δυναμική συσκευασία](media-services-dynamic-packaging-overview.md): HLS, MPEG-ΠΑΎΛΑ, ομαλή ροή και σκληροί ΔΊΣΚΟΙ. Για παράδειγμα:

Διεύθυνση URL ΠΑΎΛΑΣ MPEG με φίλτρο

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Ομαλή ροή URL με φίλτρο

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Για περισσότερες πληροφορίες σχετικά με το πώς να κάνετε το περιεχόμενό σας και να δημιουργήσετε ροής διευθύνσεις URL, ανατρέξτε στο θέμα [Επισκόπηση περιεχομένου παράδοση](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Σημειώστε ότι δυναμικές δηλώσεις δεν αλλάζουν περιουσιακού στοιχείου και τη δήλωση προεπιλογή για το συγκεκριμένο περιουσιακών στοιχείων. Να επιλέξετε το πρόγραμμα-πελάτη για να ζητήσετε μια ροή με ή χωρίς φίλτρα. 


###<a id="filters"></a>Φίλτρα 

Υπάρχουν δύο τύποι φίλτρων περιουσιακών στοιχείων: 

- Καθολικά φίλτρα (μπορούν να εφαρμοστούν σε οποιαδήποτε περιουσιακών στοιχείων στο λογαριασμό των υπηρεσιών Azure Media Services, έχει μια χρονική διάρκεια του λογαριασμού) και 
- Τοπική φίλτρα (μπορούν να εφαρμοστούν μόνο σε με τον οποίο το φίλτρο έχει συσχετιστεί με τη δημιουργία περιουσιακού στοιχείου, έχουν μια διάρκεια ζωής του περιουσιακού στοιχείου). 

Οι τύποι φίλτρου καθολικού και τοπικού έχουν ακριβώς τις ίδιες ιδιότητες. Η κύρια διαφορά μεταξύ των δύο είναι για ποια σενάρια είναι πιο κατάλληλη τι τύπο μια φίλτρων. Καθολικά φίλτρα είναι γενικά κατάλληλο για τη συσκευή προφίλ (απόδοσης φιλτράρισμα) όπου μπορεί να χρησιμοποιηθεί τοπική φίλτρα για αποκοπή ενός συγκεκριμένου περιουσιακού στοιχείου.


##<a id="scenarios"></a>Κοινά σενάρια 

Όπως αναφέρθηκε προηγουμένως, κατά την παράδοση του περιεχομένου σε πελάτες (ροή ζωντανή συμβάντα ή video-on-demand) στόχος σας είναι να παραδώσετε βίντεο υψηλής ποιότητας σε διάφορες συσκευές υπό όρους διαφορετικό δίκτυο. Επιπλέον, σας μπορεί να έχουν άλλες απαιτήσεις που αφορούν φιλτράρισμα τους πόρους σας και η χρήση του **Δυναμικού δήλωσης**s. Οι παρακάτω ενότητες παρέχουν μια σύντομη επισκόπηση των διαφορετικά σενάρια φιλτραρίσματος.

- Καθορίστε μόνο ένα υποσύνολο των αποδόσεων ήχου και βίντεο που μπορεί να διαχειριστεί ορισμένες συσκευές (αντί για όλες τις αποδόσεις που συσχετίζονται με το περιουσιακών στοιχείων). 
- Αναπαραγωγή μιας ενότητας του βίντεο (αντί για αναπαραγωγή του βίντεο ολόκληρη).
- Προσαρμογή του παραθύρου παρουσίασης DVR.

##<a name="rendition-filtering"></a>Φιλτράρισμα απόδοσης 

Μπορείτε να επιλέξετε να κωδικοποιήσετε σας περιουσιακού στοιχείου για πολλών προφίλ κωδικοποίησης (γραμμή βάσης H.264, H.264 υψηλό, AACL, AACH, Dolby ψηφιακή συν) και πολλές bitrates ποιότητα. Ωστόσο, όχι σε όλες τις συσκευές-πελάτες θα υποστηρίζει το πάγιο προφίλ και bitrates. Για παράδειγμα, παλαιότερες συσκευές Android υποστηρίζει μόνο H.264 γραμμής βάσης + AACL. Αποστολή υψηλότερη bitrates σε μια συσκευή που δεν είναι δυνατό να λάβει τα οφέλη, απορρίμματα υπολογισμού εύρους ζώνης και συσκευή. Η συσκευή πρέπει να αποκωδικοποιείτε όλες τις πληροφορίες που δίνονται, μόνο για να κλιμακωθεί προς τα κάτω για εμφάνιση.

Με δυναμική δήλωσης, μπορείτε να δημιουργήσετε προφίλ συσκευών όπως mobile, console, HD/SD, κ.λπ., και περιλαμβάνουν τα κομμάτια και τις ιδιότητες που θέλετε να είναι μέρος κάθε προφίλ.

 
![Παράδειγμα φιλτραρίσματος απόδοσης][renditions2]

Στο παρακάτω παράδειγμα, μια κωδικοποιητή που χρησιμοποιήθηκε για την κωδικοποίηση ενός περιουσιακού στοιχείου θυγατρική σε επτά ISO MP4s βίντεο αποδόσεις (από 180p για να 1080p). Κωδικοποιημένο παγίου να δυναμικά συσκευαστούν σε οποιοδήποτε από τα ακόλουθα πρωτόκολλα ροής: HLS, ομαλά, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ.  Στο επάνω μέρος του διαγράμματος, εμφανίζεται το δηλωτικό HLS του παγίου με χωρίς φίλτρα (περιέχει όλες τις αποδόσεις επτά).  Στο κάτω αριστερά, εμφανίζεται το δηλωτικό HLS στο οποίο έχει εφαρμοστεί ένα φίλτρο με το όνομα "ott". Καθορίζει το φίλτρο "ott" για να καταργήσετε όλα bitrates κάτω από το 1Mbps, η οποία οδήγησε σε τα επίπεδα ποιότητας δύο κάτω που αφαιρούνται στην απάντηση.  Στην κάτω δεξιά, εμφανίζεται το δηλωτικό HLS στο οποίο έχει εφαρμοστεί ένα φίλτρο με το όνομα "κινητές συσκευές". Καθορίζει το φίλτρο "κινητές συσκευές" για να καταργήσετε αποδόσεις όπου η λύση είναι μεγαλύτερο από 720p, που οδήγησε σε δύο 1080p αποδόσεις που αφαιρούνται.

![Φιλτράρισμα απόδοσης][renditions1]

##<a name="removing-language-tracks"></a>Κατάργηση κομμάτια γλώσσας

Τους πόρους σας ενδέχεται να περιλαμβάνουν πολλές γλώσσες ήχου όπως Αγγλικά, Ισπανικά, γαλλικά, κ.λπ. Συνήθως, τους διευθυντές SDK προγράμματος αναπαραγωγής κομματιού από προεπιλογή και παρακολουθεί τις διαθέσιμες ήχου ανά χρήστη επιλογής. Είναι ενδιαφέρουσα για την ανάπτυξη όπως SDK Player, απαιτεί διαφορετικές υλοποιήσεις σε συγκεκριμένη συσκευή αναπαραγωγής-πλαισίων. Επίσης, σε ορισμένες πλατφόρμες, Player APIs περιορίζονται και μην συμπεριλαμβάνετε τη δυνατότητα επιλογής ήχου όπου οι χρήστες δεν είναι δυνατό να επιλέξετε ή να αλλάξετε το προεπιλεγμένο κομμάτι ήχου. Με τα φίλτρα περιουσιακών στοιχείων, μπορείτε να ελέγξετε τη συμπεριφορά κατά τη δημιουργία φίλτρων που περιλαμβάνει μόνο ήχου γλώσσα που θέλετε.

![Φιλτράρισμα κομμάτια γλώσσας][language_filter]


##<a name="trimming-start-of-an-asset"></a>Περικοπή Έναρξη ενός περιουσιακού στοιχείου 

Στα περισσότερα ζωντανή ροή συμβάντα, τελεστές εκτελούνται ορισμένες δοκιμών πριν από την πραγματική συμβάντος. Για παράδειγμα, ενδέχεται να περιλαμβάνουν έναν κατάλογο ως εξής πριν από την έναρξη του συμβάντος: "Το πρόγραμμα θα ξεκινήσει στιγμιαία". Εάν το πρόγραμμα αρχειοθέτηση, η δοκιμή και κατάλογο δεδομένων αρχειοθετούνται επίσης και θα συμπεριληφθούν στην παρουσίαση. Ωστόσο, αυτές οι πληροφορίες δεν θα πρέπει να εμφανίζεται στα προγράμματα-πελάτες. Με δυναμική δήλωσης, μπορείτε να δημιουργήσετε ένα φίλτρο ώρα έναρξης και να καταργήσετε τα ανεπιθύμητα δεδομένα από το δηλωτικό.

![Έναρξη περικοπής][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Δημιουργία δευτερευουσών αποσπάσματα (προβολές) από μια ζωντανή αρχειοθήκη

Πολλά ζωντανή συμβάντα είναι μεγάλες εκτελείται και ζωντανή αρχειοθέτησης ενδέχεται να περιλαμβάνουν πολλές συμβάντα. Μετά την εκδήλωση ζωντανή οργανισμών τελειώνει μπορεί να θέλετε να διασπάσετε την ζωντανή αρχειοθέτηση σε λογικές πρόγραμμα εκκίνησης και διακοπή ακολουθίες. Στη συνέχεια, δημοσίευση εικονικού αυτά τα προγράμματα ξεχωριστά χωρίς δημοσίευση επεξεργασίας στην αρχειοθήκη live και δεν τη δημιουργία ξεχωριστά στοιχεία (το οποίο δεν θα σας βοηθήσουν πλεονέκτημα της του υπάρχοντος τμημάτων στο cache σε το CDN). Παραδείγματα αυτά τα προγράμματα εικονικού (δευτερεύουσες αποσπάσματα) είναι τα τρίμηνα μια ποδοσφαίρου ή παιχνίδι μπάσκετ, το innings στο μπέιζμπολ ή μεμονωμένα συμβάντα από ένα απόγευμα του προγράμματος Ολυμπιακούς αγώνες.

Με δυναμική δήλωσης, μπορείτε να δημιουργήσετε φίλτρα χρησιμοποιώντας ώρα έναρξης/λήξης και να Δημιουργία εικονικών προβολών στο επάνω μέρος του live αρχειοθέτησης. 

![Φίλτρο subclip][subclip_filter]

Φιλτραρισμένες περιουσιακών στοιχείων:

![Σκι][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Προσαρμογή του παραθύρου παρουσίασης (DVR)

Προς το παρόν, των υπηρεσιών Azure Media Services προσφέρει κυκλική αρχειοθέτησης όπου μπορεί να ρυθμιστεί τη διάρκεια μεταξύ 5 λεπτά - 25 ώρες. Φιλτράρισμα δήλωσης μπορεί να χρησιμοποιηθεί για να δημιουργήσετε ένα παράθυρο συνάθροισης DVR επάνω από το επάνω μέρος του αρχείου, χωρίς να διαγράψει πολυμέσων. Υπάρχουν πολλά σενάρια όπου οργανισμών θέλετε να δώσετε ένα περιορισμένο παράθυρο DVR ποια μετακινείται με το πραγματικό άκρο και την ίδια στιγμή διατηρείτε ένα μεγαλύτερο παράθυρο αρχειοθέτησης. Μια προέλευση της εκπομπής μπορεί να θέλετε να χρησιμοποιήσετε τα δεδομένα που βρίσκονται έξω από το παράθυρο DVR για να επισημάνετε τα αποσπάσματα ή he\she μπορεί να θέλετε να παράσχετε διαφορετικά παράθυρα DVR για διαφορετικές συσκευές. Για παράδειγμα, οι περισσότερες από τις κινητές συσκευές δεν μπορούν να χειριστούν μεγάλο DVR windows (μπορείτε να έχετε ένα παράθυρο DVR 2 λεπτό για κινητές συσκευές και 1 ώρα για προγράμματα-πελάτες υπολογιστή).

![Παράθυρο DVR][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Προσαρμογή LiveBackoff (ζωντανή θέση)

Φιλτράρισμα δήλωσης μπορεί να χρησιμοποιηθεί για να καταργήσετε μερικά δευτερόλεπτα από το live άκρο του live προγράμματος. Αυτό σας επιτρέπει οργανισμών να παρακολουθήσουν την παρουσίαση του σημείου δημοσίευσης προεπισκόπηση και να δημιουργήσετε διαφήμιση σημεία εισαγωγής πριν από τα προγράμματα προβολής λάβουν τη ροή (συνήθως αντίγραφα-απενεργοποίηση από 30 δευτερόλεπτα). Οργανισμών, στη συνέχεια, να προωθήσετε αυτές τις διαφημίσεις σε τους πλαίσια προγράμματος-πελάτη σε χρόνο για να received και να επεξεργαστείτε τις πληροφορίες πριν από την ευκαιρία διαφήμιση.

Εκτός από την υποστήριξη διαφήμιση, LiveBackoff μπορεί να χρησιμοποιηθεί για την προσαρμογή θέση ζωντανή λήψη προγράμματος-πελάτη, έτσι ώστε όταν οι υπολογιστές-πελάτες μετατοπίζεται και πατήσετε το πραγματικό άκρο μπορεί να αποκτήσει ακόμη τμήματα από το διακομιστή αντί να εμφανίζονται σφάλματα HTTP 404 ή 412.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Συνδυασμός πολλών κανόνων σε ένα φίλτρο

Μπορείτε να συνδυάσετε πολλούς κανόνες φιλτραρίσματος σε ένα φίλτρο. Ως παράδειγμα, μπορείτε να ορίσετε έναν κανόνα περιοχή για να καταργήσετε slate από μια ζωντανή αρχειοθήκη και να φιλτράρουν επίσης διαθέσιμες bitrates. Για πολλούς κανόνες φιλτραρίσματος το τελικό αποτέλεσμα είναι η σύνθεση (μόνο Διασταύρωση) από αυτούς τους κανόνες.

![κανόνες πολλαπλάσιο][multiple-rules]

##<a name="create-filters-programmatically"></a>Δημιουργία φίλτρων μέσω προγραμματισμού

Το παρακάτω θέμα ασχολείται με οντοτήτων Media Services που σχετίζονται με τα φίλτρα. Το θέμα εμφανίζει επίσης τον τρόπο δημιουργίας φίλτρων μέσω προγραμματισμού.  

[Δημιουργία φίλτρων με REST API](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Συνδυασμός πολλών φίλτρων (φιλτραρίσματος σύνθεσης)

Μπορείτε επίσης να συνδυάσετε πολλά φίλτρα σε μια μεμονωμένη διεύθυνση URL. 

Το ακόλουθο σενάριο παρουσιάζει γιατί μπορεί να θέλετε να συνδυάσετε τα φίλτρα:

1. Πρέπει να φιλτράρετε τις ιδιότητες βίντεο για κινητές συσκευές, όπως Android ή το iPAD (για να περιορίσετε τις ιδιότητες βίντεο). Για να καταργήσετε τα ανεπιθύμητα ιδιότητες, πρέπει να δημιουργήσετε καθολικό φίλτρο που είναι κατάλληλη για τη συσκευή προφίλ. Όπως προαναφέρθηκε, καθολικά φίλτρα μπορεί να χρησιμοποιηθεί για όλους τους πόρους σας στην περιοχή τον ίδιο λογαριασμό υπηρεσίες πολυμέσων χωρίς συσχετισμό περαιτέρω. 
2. Μπορείτε επίσης να θέλετε να αποκόψετε την ώρα έναρξης και λήξης ενός περιουσιακού στοιχείου. Για να επιτύχετε αυτό, θα δημιουργήσετε ένα τοπικό φίλτρο και να ορίσετε την ώρα έναρξης/λήξης. 
3. Θέλετε να συνδυάσετε και τα δύο από αυτά τα φίλτρα (χωρίς συνδυασμό που χρειάζεστε για να προσθέσετε ποιότητα φιλτράρισμα στο φίλτρο περικοπής που θα κάνει χρήση φίλτρου δύσκολη).

Για να συνδυάσετε τα φίλτρα, πρέπει να ορίσετε τα ονόματα φίλτρο στη λίστα δηλωτικό/αναπαραγωγής διεύθυνση URL με ελληνικό ερωτηματικό με κόμματα. Ας υποθέσουμε ότι έχετε ένα φίλτρο που ονομάζεται *MyMobileDevice* ότι ιδιότητες φίλτρα και έχετε άλλο συγκεκριμένο *MyStartTime* για να ορίσετε μια συγκεκριμένη ώρα έναρξης. Μπορείτε να συνδυάσετε τους ως εξής:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Μπορείτε να συνδυάσετε έως 3 φίλτρα. 

Για περισσότερες πληροφορίες, ανατρέξτε [σε αυτό](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) το ιστολόγιο.


##<a name="know-issues-and-limitations"></a>Γνωρίζετε θέματα και περιορισμούς

- Δυναμική δηλωτικό λειτουργεί σε GOP όρια (πλήκτρο πλαίσια), ως εκ τούτου περικοπή έχει GOP ακρίβειας. 
- Μπορείτε να χρησιμοποιήσετε το ίδιο όνομα φίλτρου για τοπική και καθολικά φίλτρα. Σημείωση το τοπικό φίλτρο έχουν υψηλότερη προτεραιότητα και θα αντικαταστήσει καθολικά φίλτρα.
- Εάν ενημερώνετε ένα φίλτρο, αυτό μπορεί να διαρκέσει έως και 2 λεπτά για ροή τελικό σημείο για την ανανέωση των κανόνων. Εάν το περιεχόμενο έχει που σερβιρίστηκε με ορισμένα φίλτρα (και στο cache σε διακομιστές μεσολάβησης και CDN μνήμης cache), ενημέρωση αυτά τα φίλτρα μπορεί να προκαλέσει αποτυχίες προγράμματος αναπαραγωγής. Είναι συνιστάται να απαλείψετε το cache μετά την ενημέρωση του φίλτρου. Εάν αυτή η επιλογή δεν είναι δυνατό, μπορείτε να χρησιμοποιήσετε ένα διαφορετικό φίλτρο.


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Δείτε επίσης

[Παράδοση περιεχομένου στην Επισκόπηση πελάτες](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 