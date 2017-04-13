<properties 
    pageTitle="Εισαγωγή διαφημίσεις στην πλευρά του προγράμματος-πελάτη | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να εισαγάγετε διαφημίσεις στην πλευρά του προγράμματος-πελάτη." 
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


#<a name="inserting-ads-on-the-client-side"></a>Εισαγωγή διαφημίσεις στην πλευρά του προγράμματος-πελάτη

Αυτό το θέμα περιέχει πληροφορίες σχετικά με τον τρόπο εισαγωγής τους διάφορους τύπους διαφημίσεις στην πλευρά του προγράμματος-πελάτη.

Για πληροφορίες σχετικά με κλειστές λεζάντες και ad υποστήριξη στο Live ροής βίντεο, ανατρέξτε στο θέμα [υποστηριζόμενες κλειστές λεζάντες και πρότυπα εισαγωγής Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player δεν υποστηρίζει τη συγκεκριμένη στιγμή διαφημίσεις.

##<a id="insert_ads_into_media"></a>Εισαγωγή διαφημίσεις σε τα πολυμέσα

Azure Media Services παρέχει υποστήριξη για εισαγωγής ad μέσω της πλατφόρμας Windows Media: πλαίσια προγράμματος αναπαραγωγής. Πλαίσια προγράμματος αναπαραγωγής με την υποστήριξη ad είναι διαθέσιμες για συσκευές Windows 8, Silverlight, Windows Phone 8 και iOS. Κάθε πλαίσιο προγράμματος αναπαραγωγής περιέχει δείγμα κώδικα που δείχνει πώς μπορείτε να υλοποιήσετε μια εφαρμογή του προγράμματος αναπαραγωγής. Υπάρχουν τρία διαφορετικά είδη διαφημίσεις, μπορείτε να εισαγάγετε στο πολυμέσων: λίστα σας.

- **Αριθμητική πρόοδος** – διαφημίσεων πλήρους πλαισίου που παύση του κύριου βίντεο.
- **Nonlinear** – διαφημίσεις επικάλυψης που εμφανίζονται όπως γίνεται αναπαραγωγή του βίντεο κύριο, συνήθως ένα λογότυπο ή άλλη στατική εικόνα τοποθετείται εντός του προγράμματος αναπαραγωγής.
- **Συνοδευτικό** – διαφημίσεων που εμφανίζονται εκτός του προγράμματος αναπαραγωγής.

Διαφημίσεις μπορούν να τεθούν σε οποιοδήποτε σημείο στη λωρίδα χρόνου στο κύριο βίντεο. Πρέπει να ενημερώσετε το πρόγραμμα αναπαραγωγής πότε πρέπει να αναπαραγάγετε το ad και τις διαφημίσεις για την αναπαραγωγή. Αυτό γίνεται χρησιμοποιώντας ένα σύνολο τυπικά αρχεία που βασίζονται σε XML: πρότυπο υπηρεσιών Ad βίντεο (VAST), ψηφιακή βίντεο πολλών Ad αναπαραγωγής (VMAP), πρότυπο αποσπάσματος ακολουθιακή διαχείριση στο πολυμέσων (ΚΎΡΙΑΣ) και ψηφιακού βίντεο Player Ad περιβάλλοντος εργασίας Definition (VPAID). ΤΕΡΆΣΤΙΕΣ αρχείων Καθορίστε ποιες διαφημίσεις για να εμφανίσετε. Αρχεία VMAP να καθορίσετε πότε πρέπει να αναπαραγάγετε διάφορες διαφημίσεις και περιέχουν ΤΕΡΆΣΤΙΕΣ XML. Τα αρχεία ΚΎΡΙΑΣ είναι ένας άλλος τρόπος για να διαφημίσεις ακολουθίας που μπορεί να περιέχει επίσης ΤΕΡΆΣΤΙΕΣ XML. Αρχεία VPAID ορίζουν ένα περιβάλλον εργασίας μεταξύ του προγράμματος αναπαραγωγής βίντεο και το ad ή διακομιστής ad.

Κάθε πλαίσιο προγράμματος αναπαραγωγής λειτουργεί διαφορετικά και κάθε θα καλύπτονται στο δικό του θέματος. Αυτό το θέμα θα περιγράφονται οι βασικές μηχανισμοί που χρησιμοποιούνται για να εισαγάγετε διαφημίσεις. Εφαρμογές του προγράμματος αναπαραγωγής βίντεο αίτηση διαφημίσεις από ένα διακομιστή ad. Ο διακομιστής ad να απαντήσετε με διάφορους τρόπους:

- Επιστροφή ΤΕΡΆΣΤΙΕΣ αρχείου
- Επιστρέφει ένα αρχείο VMAP (με ενσωματωμένο VAST)
- Επιστρέφει ένα αρχείο ΚΎΡΙΑΣ (με ενσωματωμένο VAST)
- Επιστροφή ΤΕΡΆΣΤΙΕΣ αρχείου με διαφημίσεις VPAID

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Χρησιμοποιώντας ένα αρχείο βίντεο Ad υπηρεσίας πρότυπο (VAST)

Ένα αρχείο ΤΕΡΆΣΤΙΕΣ καθορίζει ποιες ad ή διαφημίσεις για να εμφανίσετε. Το παρακάτω δείγμα XML είναι ένα παράδειγμα ενός ΤΕΡΆΣΤΙΕΣ αρχείου για μια γραμμική ad:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Η γραμμική ad περιγράφεται από το **<Linear>** στοιχείο. Καθορίζει τη διάρκεια της το ad, παρακολούθηση συμβάντων, κάντε κλικ στην επιλογή, κάντε κλικ στην επιλογή παρακολούθηση και έναν αριθμό **<MediaFile>** στοιχεία. Παρακολούθηση συμβάντων καθορίζονται εντός του **<TrackingEvents>** στοιχείο και να επιτρέψετε σε ένα διακομιστή ad για να παρακολουθείτε διάφορα συμβάντα που προκύπτουν κατά την προβολή του ad. Σε αυτήν την περίπτωση Έναρξη, μέση, ολοκλήρωσης, και αναπτύξτε το στοιχείο παρακολουθούνται συμβάντα. Όταν εμφανίζεται το ad που προκύπτει το συμβάν έναρξης. Το κεντρικό σημείο συμβάν πραγματοποιείται όταν τουλάχιστον 50% της λωρίδας χρόνου του ad έχει προβληθεί. Το συμβάν ολοκληρωθεί εμφανίζεται όταν το ad έχει εκτελεστεί στο τέλος. Το συμβάν ανάπτυξη πραγματοποιείται όταν ο χρήστης επεκτείνει το πρόγραμμα αναπαραγωγής βίντεο σε πλήρη οθόνη. Clickthroughs καθορίζονται με ένα **<ClickThrough>** από μια **<VideoClicks>** στοιχείο και καθορίζει ένα URI σε έναν πόρο για να εμφανίζονται όταν ο χρήστης κάνει κλικ σε το ad. ClickTracking έχει καθοριστεί σε μια **<ClickTracking>** στοιχείο, επίσης μέσα το **<VideoClicks>** στοιχείο και καθορίζει έναν πόρο παρακολούθησης για το πρόγραμμα αναπαραγωγής για να ζητήσετε όταν ο χρήστης κάνει κλικ σε το ad. Το **<MediaFile>** στοιχεία καθορίζουν τις πληροφορίες σχετικά με μια συγκεκριμένη κωδικοποίηση μια διαφήμιση. Όταν υπάρχει περισσότερες από μία **<MediaFile>** στοιχείο, το πρόγραμμα αναπαραγωγής βίντεο να επιλέξετε την καλύτερη κωδικοποίηση για την πλατφόρμα. 

Γραμμική διαφημίσεις μπορεί να εμφανίζεται σε μια καθορισμένη σειρά. Για να κάνετε αυτό, προσθέστε επιπλέον <Ad> στοιχεία για να τα VAST αρχείων και καθορίστε τη σειρά χρησιμοποιώντας το χαρακτηριστικό ακολουθίας. Το ακόλουθο παράδειγμα δείχνει αυτό:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Μη γραμμικά διαφημίσεις καθορίζονται σε ένα <Creative> καθώς και το στοιχείο. Στο ακόλουθο παράδειγμα μιας <Creative> στοιχείο που περιγράφει μια γραμμική ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
Το **<NonLinearAds>** στοιχείο μπορεί να περιέχει μία ή περισσότερες **<NonLinear>** στοιχεία, κάθε μία από τις οποίες μπορούν να περιγράψετε μια γραμμική ad. Το **<NonLinear>** στοιχείο καθορίζει τον πόρο για την ad μη γραμμικά. Ο πόρος μπορεί να είναι ένα **<StaticResouce>**, μια **<IFrameResource>**, ή μια **<HTMLResouce>**.**<StaticResource>** Περιγράφει έναν πόρο μη HTML και ορίζει ένα χαρακτηριστικό creativeType που καθορίζει τον τρόπο εμφάνισης τον πόρο:

Εικόνα/gif, εικόνα/jpeg, εικόνα/png – ο πόρος εμφανίζεται σε μια μορφή HTML **<img>** ετικέτα.

Εφαρμογή/x-javascript – ο πόρος εμφανίζεται σε μια ετικέτα HTML <**δέσμης ενεργειών**>.

Εφαρμογή/x-shockwave-flash – ο πόρος εμφανίζεται σε μια Flash player.

**<IFrameResource>**Περιγράφει έναν πόρο HTML που μπορούν να εμφανιστούν σε ένα IFrame. **<HTMLResource>**Περιγράφει ένα τμήμα κώδικα HTML που μπορεί να εισαχθεί σε μια ιστοσελίδα. **<TrackingEvents>**Καθορίστε παρακολούθηση συμβάντων και URI για να ζητήσετε όταν εμφανίζεται το συμβάν. Σε αυτό το δείγμα παρακολουθούνται τα συμβάντα acceptInvitation και σύμπτυξη. Για περισσότερες πληροφορίες σχετικά με το **<NonLinearAds>** στοιχείο και στα θυγατρικά στοιχεία, ανατρέξτε στο θέμα IAB.NET/VAST. Λάβετε υπόψη ότι η **<TrackingEvents>** στοιχείο βρίσκεται μέσα σε το** <NonLinearAds> ** στοιχείο αντί για το **<NonLinear>** στοιχείο.

Συνοδευτικό διαφημίσεις ορίζονται μέσα σε μια <CompanionAds> στοιχείο. Το <CompanionAds> στοιχείο μπορεί να περιέχει μία ή περισσότερες <Companion> στοιχεία. Κάθε <Companion> στοιχείο περιγράφει μια συνοδευτική ad και μπορεί να περιέχει ένα <StaticResource>, <IFrameResource>, ή <HTMLResource> που έχουν καθοριστεί με τον ίδιο τρόπο όπως στο μια γραμμική ad. ΤΕΡΆΣΤΙΕΣ αρχείου μπορεί να περιέχει πολλές συνοδευτικό διαφημίσεις και την εφαρμογή του προγράμματος αναπαραγωγής να επιλέξετε το πιο κατάλληλο ad για να εμφανίσετε. Για περισσότερες πληροφορίες σχετικά με το VAST, ανατρέξτε στο θέμα [ΤΕΡΆΣΤΙΕΣ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Χρήση ψηφιακών βίντεο πολλών αρχείου αναπαραγωγής (VMAP) Ad

Ένα αρχείο VMAP σας επιτρέπει να καθορίσετε όταν προκύπτουν ad αλλαγές, για πόσο καιρό είναι κάθε αλλαγή, πόσες διαφημίσεις μπορούν να εμφανιστούν μέσα σε μια αλλαγή και ποιοι τύποι διαφημίσεις ενδέχεται να εμφανίζεται κατά τη διάρκεια της αλλαγής. Τα εξής στο παράδειγμα αρχείου VMAP που καθορίζει μια αλλαγή μία ad:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Ένα αρχείο VMAP αρχίζει με μια <VMAP> στοιχείο που περιέχει έναν ή περισσότερους <AdBreak> στοιχεία, κάθε καθορίζει μια αλλαγή ad. Κάθε αλλαγή ad καθορίζει έναν τύπο αλλαγής Αναγνωριστικό αλλαγής και χρονική μετάθεση. Το χαρακτηριστικό breakType καθορίζει τον τύπο του ad που μπορεί να γίνει κατά την αλλαγή: γραμμική, μη γραμμικό, ή να εμφανίσετε. Εμφάνιση χάρτη διαφημίσεις για να ΤΕΡΆΣΤΙΕΣ συνοδευτικό διαφημίσεις. Περισσότερους από έναν τύπους ad μπορεί να καθοριστεί σε μια λίστα διαχωρισμένων με κόμματα (χωρίς κενά διαστήματα). Το breakID είναι ένα προαιρετικό αναγνωριστικό για την ad. Η timeOffset Καθορίζει πότε θα πρέπει να εμφανίζεται το ad. Αυτό μπορεί να έχει καθοριστεί σε έναν από τους εξής τρόπους:

1. Χρόνος – σε μορφή hh:mm:ss ή hh:mm:ss.mmm όπου .mmm είναι χιλιοστά του δευτερολέπτου. Η τιμή του χαρακτηριστικού αυτή καθορίζει την ώρα από την αρχή της λωρίδας χρόνου βίντεο στην αρχή της αλλαγής ad.
1. Ποσοστό – σε μορφή n % όπου n είναι το ποσοστό της λωρίδας χρόνου βίντεο για αναπαραγωγή πριν από την αναπαραγωγή του ad
1. Έναρξη/Λήξη-Καθορίζει ότι πρέπει να εμφανίζεται μια διαφήμιση πριν ή μετά την έχει εμφανιστεί το βίντεο
1. Τοποθετήστε – Καθορίζει τη σειρά των ad αλλαγές όταν το χρονισμό αλλαγές ad είναι άγνωστη, όπως η ροή live. Η σειρά της κάθε αλλαγής ad καθορίζεται στη μορφή #n όπου n είναι ένας ακέραιος αριθμός 1 ή μεγαλύτερη. 1 υποδεικνύει το ad θα πρέπει να γίνει με την πρώτη ευκαιρία, 2 υποδεικνύει το ad θα πρέπει να γίνει αναπαραγωγή με την ευκαιρία δεύτερο και ούτω καθεξής.

Μέσα στο στοιχείο <**AdBreak**> μπορεί να είναι ένα στοιχείο <**AdSource**>. Το στοιχείο <**AdSource**> περιλαμβάνει τα παρακάτω χαρακτηριστικά:

1. Αναγνωριστικό – καθορίζει ένα αναγνωριστικό για την προέλευση ad
1. allowMultipleAds – μια δυαδική τιμή που καθορίζει αν πολλές διαφημίσεις μπορούν να εμφανιστούν κατά την αλλαγή ad
1. followRedirects – προαιρετική δυαδική τιμή που καθορίζει αν θα πρέπει να εκτελέσει το πρόγραμμα αναπαραγωγής βίντεο ανακατευθύνει μέσα σε μια απόκριση ad

Το στοιχείο <**AdSource**> παρέχει το πρόγραμμα αναπαραγωγής μια ενσωματωμένη απάντηση ad ή μια αναφορά σε μια απόκριση ad. Μπορεί να περιέχει ένα από τα εξής στοιχεία:

- <VASTAdData>Υποδεικνύει μια απάντηση ΤΕΡΆΣΤΙΕΣ ad είναι ενσωματωμένο μέσα στο αρχείο VMAP
- <AdTagURI>ένα URI που αναφέρεται σε μια απόκριση ad από άλλο σύστημα
- <CustomAdData>-μια αυθαίρετη συμβολοσειρά που respresents μη ΤΕΡΆΣΤΙΕΣ απόκριση

Σε αυτό το παράδειγμα, μια απάντηση σε γραμμή ad καθορίζεται με μια <VASTAdData> στοιχείο που περιέχει μια απάντηση ΤΕΡΆΣΤΙΕΣ ad. Για περισσότερες πληροφορίες σχετικά με τα άλλα στοιχεία, ανατρέξτε στο θέμα [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Το στοιχείο <**AdBreak**> μπορεί επίσης να περιέχει ένα στοιχείο <**TrackingEvents**>. Το στοιχείο <**TrackingEvents**> σάς επιτρέπει να παρακολουθείτε την έναρξη ή το τέλος μιας αλλαγής ad ή εάν Παρουσιάστηκε σφάλμα κατά την αλλαγή ad. Το στοιχείο <**TrackingEvents**> περιέχει μία ή περισσότερες <**παρακολούθησης**> στοιχεία, τα οποία καθορίζει ένα συμβάν παρακολούθησης και παρακολούθησης URI. Τα συμβάντα πιθανές παρακολούθησης είναι:

1. breakStart – παρακολουθεί την αρχή μιας αλλαγής ad
1. breakEnd – παρακολουθούν την ολοκλήρωση μιας αλλαγής ad
1. Σφάλμα – παρακολουθεί σφάλμα που προέκυψε κατά την αλλαγή ad

Το παρακάτω παράδειγμα εμφανίζει ένα αρχείο VMAP που καθορίζει την παρακολούθηση συμβάντων

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Για περισσότερες πληροφορίες σχετικά με το στοιχείο <**TrackingEvents**> και στα θυγατρικά στοιχεία, ανατρέξτε στο θέμα http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Χρησιμοποιώντας ένα απόσπασμα πολυμέσων ακολουθιακή διαχείριση αρχείο προτύπου (ΚΎΡΙΑΣ)

Ένα αρχείο ΚΎΡΙΑΣ σάς επιτρέπει να καθορίσετε εναύσματα που ορίζουν όταν εμφανίζεται μια διαφήμιση. Ακολουθεί ένα παράδειγμα αρχείου ΚΎΡΙΑΣ που περιέχει εναυσμάτων για μια διαφήμιση άλμπουμ pre, μια διαφήμιση μέσο μιας άλμπουμ και μια διαφήμιση μετά τη άλμπουμ.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Ένα αρχείο ΚΎΡΙΑΣ αρχίζει με μια **<MAST>** στοιχείο που περιέχει ένα **<triggers>** στοιχείο. Το <triggers> στοιχείο περιέχει μία ή περισσότερες **<trigger>** στοιχεία που καθορίζουν πότε θα πρέπει να αναπαράγεται μια διαφήμιση. 

Το **<trigger>** στοιχείο περιέχει ένα **<startConditions>** στοιχείο η οποία καθορίζει πότε μια διαφήμιση πρέπει να ξεκινήσει η αναπαραγωγή. Το **<startConditions>** στοιχείο περιέχει μία ή περισσότερες <condition> στοιχεία. Όταν κάθε <condition> αξιολογούνται ως αληθείς ένα έναυσμα ξεκινά ή ανάκληση ανάλογα με το αν το <condition> περιέχεται μέσα σε μια **< startConditions**> ή **<endConditions>** στοιχείου αντίστοιχα. Όταν πολλές <condition> υπάρχουν στοιχεία, αντιμετωπίζονται ως μη ρητή OR, οποιαδήποτε συνθήκη την αξιολόγηση στην τιμή true θα προκαλέσει το έναυσμα για να ξεκινήσετε. <condition>στοιχεία μπορούν να ενσωματωθούν. Όταν το παιδί <condition> στοιχεία είναι προκαθορισμένες, αντιμετωπίζονται ως ένα μη ρητή και, πρέπει να υπολογιστεί όλες τις συνθήκες στην τιμή true για το έναυσμα για να ξεκινήσετε. Το <condition> στοιχείο περιλαμβάνει τα παρακάτω χαρακτηριστικά που ορίζουν τη συνθήκη: 

1. **Τύπος** – καθορίζει τον τύπο των συνθήκης, συμβάν ή ιδιότητα
1. **όνομα** -το όνομα της ιδιότητας ή συμβάντων που θα χρησιμοποιηθεί κατά την αξιολόγηση
1. **τιμή** , η τιμή που θα αξιολογείται σε σχέση με μια ιδιότητα
1. **τελεστής** – η λειτουργία για χρήση κατά την αξιολόγηση: EQ (ίσο), NEQ (δεν ισούται), GTR (μεγαλύτερο), GEQ (μεγαλύτερο ή ίσο), LT (μικρότερο του), LEQ (μικρότερο ή ίσο), MOD (λειτουργική μονάδα)

**<endConditions>**περιέχει επίσης <condition> στοιχεία. Όταν μια συνθήκη είναι true το έναυσμα γίνεται επαναφορά. Το <trigger> στοιχείο περιέχει επίσης ένα <sources> στοιχείο που περιέχει έναν ή περισσότερους <source> στοιχεία. Το <source> στοιχεία ορίζουν το URI απάντηση ad και τον τύπο της απόκρισης ad. Σε αυτό το παράδειγμα ενός URI δίνεται σε μια ΜΕΓΆΛΗ απάντηση. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Χρήση του προγράμματος αναπαραγωγής βίντεο-Ad Ορισμός περιβάλλοντος (VPAID)

VPAID είναι ένα API για την ενεργοποίηση εκτελέσιμο ad μονάδες για να επικοινωνήσετε με ένα πρόγραμμα αναπαραγωγής βίντεο. Έτσι εμπειρίες ιδιαίτερα διαδραστικές ad. Ο χρήστης να αλληλεπιδράσετε με το ad και το ad μπορούν να απαντούν σε ενέργειες που λαμβάνονται από το πρόγραμμα προβολής. Για παράδειγμα ένα ad ενδέχεται να εμφανίζονται κουμπιά που επιτρέπει στο χρήστη για να δείτε περισσότερες πληροφορίες ή μια έκδοση περισσότερο από το ad. Το πρόγραμμα αναπαραγωγής βίντεο πρέπει να υποστηρίζει το API VPAID και το εκτελέσιμο ad πρέπει να υλοποιήσετε το API. Όταν ένα πρόγραμμα αναπαραγωγής ζητάει μια διαφήμιση από ένα διακομιστή ad ο διακομιστής μπορεί να απάντηση με την απόκριση ΤΕΡΆΣΤΙΕΣ που περιέχει μια διαφήμιση VPAID.

Δημιουργείται ένα εκτελέσιμο ad στον κώδικα που πρέπει να εκτελείται σε ένα περιβάλλον χρόνου εκτέλεσης όπως Adobe Flash™ ή JavaScript που μπορεί να εκτελεστεί σε πρόγραμμα περιήγησης web. Όταν ένα διακομιστή ad επιστρέφει ΤΕΡΆΣΤΙΕΣ απόκριση που περιέχει μια διαφήμιση VPAID, η τιμή του apiFramework το χαρακτηριστικό στο το <MediaFile> στοιχείο πρέπει να είναι "VPAID". Αυτό το χαρακτηριστικό καθορίζει ότι τα περιεχόμενα ad μια διαφήμιση εκτελέσιμο VPAID. Το χαρακτηριστικό τύπος πρέπει να οριστεί με τον τύπο MIME το εκτελέσιμο αρχείο, όπως "εφαρμογή/x-shockwave-flash" ή "εφαρμογή/x-javascript". Στο ακόλουθο τμήματος κώδικα XML του <MediaFile> στοιχείου από μια ΜΕΓΆΛΗ απάντηση που περιέχει μια διαφήμιση εκτελέσιμο VPAID. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Ένα εκτελέσιμο ad μπορεί να γίνει προετοιμασία με χρήση του <AdParameters> από το <Linear> ή <NonLinear> στοιχεία σε μια ΜΕΓΆΛΗ απάντηση. Για περισσότερες πληροφορίες σχετικά με το <AdParameters> στοιχείο, ανατρέξτε στο θέμα [ΤΕΡΆΣΤΙΕΣ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Για περισσότερες πληροφορίες σχετικά με το API VPAID, ανατρέξτε στο θέμα [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Εφαρμογή του προγράμματος αναπαραγωγής του Windows Phone 8 με την υποστήριξη Ad με Windows ή

Την πλατφόρμα μέσο της Microsoft: Player Framework για τα Windows 8 και Windows Phone 8 περιέχει μια συλλογή από δείγματα εφαρμογών που σας δείχνουν πώς μπορείτε να υλοποιήσετε μια εφαρμογή προγράμματος αναπαραγωγής βίντεο χρησιμοποιώντας το πλαίσιο. Μπορείτε να κάνετε λήψη σε πλαίσιο προγράμματος αναπαραγωγής και τα δείγματα από το [πρόγραμμα αναπαραγωγής Framework για Windows 8 και Windows Phone 8](https://playerframework.codeplex.com).

Όταν ανοίγετε τη λύση Microsoft.PlayerFramework.Xaml.Samples θα δείτε έναν αριθμό φακέλων μέσα στο έργο. Ο φάκελος διαφημίσεις περιέχει το δείγμα κώδικα σχέση με τη δημιουργία ενός προγράμματος αναπαραγωγής βίντεο με την υποστήριξη ad. Εντός του διαφημίσεις φακέλου είναι ένας αριθμός XAML/στρατηγικής συνεργασίας ανά χώρα αρχεία η κάθε μία από τα οποία δείχνουν πώς μπορείτε να εισαγάγετε διαφημίσεις με διαφορετικό τρόπο. Η παρακάτω λίστα περιγράφει κάθε:

- AdPodPage.xaml δείχνει πώς μπορείτε να εμφανίσετε μια pod ad.
- AdSchedulingPage.xaml δείχνει πώς μπορείτε να προγραμματίσετε διαφημίσεις.
- FreeWheelPage.xaml δείχνει πώς μπορείτε να χρησιμοποιήσετε την προσθήκη FreeWheel για να προγραμματίσετε διαφημίσεις.
- MastPage.xaml δείχνει πώς μπορείτε να προγραμματίσετε διαφημίσεις με ένα αρχείο ΚΎΡΙΑΣ.
- ProgrammaticAdPage.xaml εμφανίζει τον τρόπο προγραμματισμού μέσω προγραμματισμού διαφημίσεις σε βίντεο.
- ScheduleClipPage.xaml δείχνει πώς μπορείτε να προγραμματίσετε μια διαφήμιση χωρίς ΤΕΡΆΣΤΙΕΣ αρχείου.
- VastLinearCompanionPage.xaml δείχνει πώς μπορείτε να εισαγάγετε μια γραμμική και ad συνοδευτικό.
- VastNonLinearPage.xaml δείχνει πώς μπορείτε να εισαγάγετε μια μη γραμμικά ad.
- VmapPage.xaml δείχνει πώς μπορείτε να καθορίσετε διαφημίσεις με ένα αρχείο VMAP.

Κάθε ένα από αυτά τα δείγματα χρησιμοποιεί την κλάση MediaPlayer που ορίζονται από το πλαίσιο του προγράμματος αναπαραγωγής. Τα περισσότερα δείγματα Χρησιμοποιήστε προσθήκες που μπορείτε να προσθέσετε υποστήριξη για διάφορες μορφές απόκριση ad. Το δείγμα ProgrammaticAdPage μέσω προγραμματισμού αλληλεπιδρά με μια περίοδο λειτουργίας MediaPlayer.

###<a name="adpodpage-sample"></a>Δείγμα AdPodPage

Αυτό το δείγμα χρησιμοποιεί το AdSchedulerPlugin για να καθορίσει πότε για να εμφανίσετε μια διαφήμιση. Σε αυτό το παράδειγμα μια διαφήμιση μέσο μιας άλμπουμ είναι προγραμματισμένη για αναπαραγωγή μετά από 5 δευτερόλεπτα. Το pod ad (μια ομάδα διαφημίσεων για να εμφανίσετε σε σειρά) έχει καθοριστεί σε ένα αρχείο ΤΕΡΆΣΤΙΕΣ που επιστρέφονται από ένα διακομιστή ad. Το URI στο αρχείο ΤΕΡΆΣΤΙΕΣ καθορίζεται στο το <RemoteAdSource> στοιχείο.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Για περισσότερες πληροφορίες σχετικά με το AdSchedulerPlugin, ανατρέξτε στο θέμα [διαφημίσεις στο πλαίσιο προγράμματος αναπαραγωγής Windows 8 και Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

Αυτό το δείγμα χρησιμοποιεί επίσης το AdSchedulerPlugin. Προγραμματίζει τρεις διαφημίσεις, μια διαφήμιση προ-άλμπουμ, μια διαφήμιση μέσο μιας άλμπουμ και μια διαφήμιση μετά τη άλμπουμ. Το URI για να το VAST για κάθε ad έχει καθοριστεί σε μια <RemoteAdSource> στοιχείο.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

Αυτό το δείγμα χρησιμοποιεί το FreeWheelPlugin που καθορίζει ένα χαρακτηριστικό προέλευσης που καθορίζει ένα URI που οδηγεί σε ένα αρχείο SmartXML που καθορίζει ad περιεχομένου, καθώς και πληροφορίες προγραμματισμού ad.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

Αυτό το δείγμα χρησιμοποιεί το MastSchedulerPlugin που σας επιτρέπει να χρησιμοποιήσετε ένα αρχείο ΚΎΡΙΑΣ. Το χαρακτηριστικό προέλευσης καθορίζει τη θέση του αρχείου ΚΎΡΙΑΣ.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

Αυτό το δείγμα μέσω προγραμματισμού αλληλεπιδρά με το MediaPlayer. Το αρχείο ProgrammaticAdPage.xaml εμφανίζει το MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Το αρχείο ProgrammaticAdPage.xaml.cs δημιουργεί μια AdHandlerPlugin, προσθέτει μια TimelineMarker για να καθορίσετε όταν μια διαφήμιση πρέπει να εμφανίζεται και, στη συνέχεια, προσθέτει ένα πρόγραμμα χειρισμού για το συμβάν MarkerReached που φορτώνει ένα RemoteAdSource που καθορίζει ένα URI ΤΕΡΆΣΤΙΕΣ σε αρχείο και, στη συνέχεια, να αναπαράγεται το ad.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

Αυτό το δείγμα χρησιμοποιεί το AdSchedulerPlugin για να προγραμματίσετε μια διαφήμιση μέσο μιας άλμπουμ, καθορίζοντας ένα αρχείο .wmv που περιέχει το ad.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Αυτό το δείγμα παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε το AdSchedulerPlugin για να προγραμματίσετε μια γραμμική ad μέσο μιας άλμπουμ με μια συνοδευτική ad. Το <RemoteAdSource> στοιχείο Καθορίζει τη θέση του αρχείου ΤΕΡΆΣΤΙΕΣ.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

Αυτό το δείγμα χρησιμοποιεί το AdSchedulerPlugin για να προγραμματίσετε μια γραμμική και μιας μη γραμμικά ad. Η θέση του αρχείου ΤΕΡΆΣΤΙΕΣ καθορίζεται με το <RemoteAdSource> στοιχείο.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Δείγματα αυτό χρησιμοποιεί τη VmapSchedulerPlugin για να προγραμματίσετε διαφημίσεις χρησιμοποιώντας ένα αρχείο VMAP. Το URI στο αρχείο VMAP έχει καθοριστεί στο χαρακτηριστικό προέλευσης του το <VmapSchedulerPlugin> στοιχείο.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Εφαρμογή ενός προγράμματος αναπαραγωγής βίντεο με την υποστήριξη Ad iOS


Η πλατφόρμα μέσο της Microsoft: Framework προγράμματος αναπαραγωγής για iOS περιέχει μια συλλογή δείγματα εφαρμογών που σας δείχνουν πώς μπορείτε να υλοποιήσετε μια εφαρμογή προγράμματος αναπαραγωγής βίντεο χρησιμοποιώντας το πλαίσιο. Μπορείτε να λάβετε το πλαίσιο προγράμματος αναπαραγωγής και τα δείγματα από [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Η σελίδα github έχει μια σύνδεση σε ένα Wiki που περιέχει πρόσθετες πληροφορίες σχετικά με το πλαίσιο του προγράμματος αναπαραγωγής και μια εισαγωγή σχετικά με το δείγμα προγράμματος αναπαραγωγής: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Προγραμματισμός διαφημίσεις με VMAP

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε διαφημίσεις χρησιμοποιώντας ένα αρχείο VMAP.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Προγραμματισμός διαφημίσεις με VAST

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια καθυστερημένη ad ΤΕΡΆΣΤΙΕΣ σύνδεσης.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια πρώιμη ad ΤΕΡΆΣΤΙΕΣ σύνδεσης.
Παράδειγμα: 4 χρονοδιάγραμμα μια πρώιμη //Download ΤΕΡΆΣΤΙΕΣ ad σύνδεσης του VAST αρχείων εάν (! [ framework.adResolver downloadManifest: & δήλωσης withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[αυτόματης logFrameworkError];} άλλο {adLinearTime.startTime = 7-adLinearTime.duration = 0.
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να εισαγάγετε μια διαφήμιση χρησιμοποιώντας πρόχειρη αποκοπή επεξεργασίας (RCE)

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια pod ad.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια μη Αυτοκόλλητες ad μέσο μιας άλμπουμ. Ένα μη Αυτοκόλλητες ad αναπαράγεται μόνο αφού ανεξάρτητα από οποιαδήποτε αναζητούν εκτελεί το πρόγραμμα προβολής.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια Αυτοκόλλητες ad μέσο μιας άλμπουμ. Μια Αυτοκόλλητες ad θα εμφανίζεται κάθε φορά που το συγκεκριμένο σημείο στη λωρίδα χρόνου βίντεο.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια διαφήμιση μετά τη άλμπουμ.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια διαφήμιση προ-άλμπουμ.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να προγραμματίσετε μια διαφήμιση μέσο μιας άλμπουμ επικάλυψης.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Δείτε επίσης

[Ανάπτυξη εφαρμογών προγράμματος αναπαραγωγής βίντεο](media-services-develop-video-players.md)
