<properties
    pageTitle="Χρήση πολλών αρχείων εισόδου και ιδιότητες στοιχείων με κωδικοποιητή Premium | Microsoft Azure"
    description="Αυτό το θέμα εξηγεί τον τρόπο χρήσης του setRuntimeProperties για να χρησιμοποιήσετε πολλά αρχεία εισόδου και μεταβιβάζουν προσαρμοσμένα δεδομένα στον επεξεργαστή πολυμέσων ροής εργασίας Premium κωδικοποιητή πολυμέσων."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Χρήση πολλών αρχείων εισόδου και ιδιότητες στοιχείων με κωδικοποιητή Premium

## <a name="overview"></a>Επισκόπηση

Υπάρχουν σενάρια στην οποία μπορεί να χρειάζεται για να προσαρμόσετε το στοιχείο ιδιότητες, καθορίστε περιεχομένου XML λίστα εικόνων Clip ή αποστολή πολλών εισαγωγής αρχείων κατά την υποβολή μιας εργασίας με τον επεξεργαστή πολυμέσων **Ροής εργασίας Premium κωδικοποιητή πολυμέσων** . Ορισμένα παραδείγματα είναι οι εξής:

- Επικάλυψη κειμένου σε βίντεο και ρύθμιση της τιμής κειμένου (για παράδειγμα, την τρέχουσα ημερομηνία) κατά το χρόνο εκτέλεσης για κάθε εισαγωγής βίντεο.
- Προσαρμογή της λίστας XML Clip (για να καθορίσετε ένα ή περισσότερα αρχεία προέλευσης, με ή χωρίς περικοπής, κ.λπ.).
- Επικάλυψη μια εικόνα λογότυπου στην είσοδο βίντεο, ενώ το βίντεο κωδικοποιείται.

Για να επιτρέψετε τη **Ροή εργασίας Premium κωδικοποιητή πολυμέσων** γνωρίζετε ότι πρόκειται να αλλάξετε ορισμένες ιδιότητες εντός της ροής εργασίας κατά τη δημιουργία της εργασίας ή αποστολή πολλών αρχείων εισόδου, θα πρέπει να χρησιμοποιήσετε μια συμβολοσειρά ρύθμισης παραμέτρων που περιέχει **setRuntimeProperties** ή/και **transcodeSource**. Αυτό το θέμα εξηγεί τον τρόπο χρήσης τους.


## <a name="configuration-string-syntax"></a>Σύνταξη συμβολοσειράς ρύθμισης παραμέτρων

Η συμβολοσειρά ρύθμισης παραμέτρων για να ορίσετε την εργασία κωδικοποίησης χρησιμοποιεί ένα έγγραφο XML που έχει την παρακάτω μορφή:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Ακολουθεί ο κώδικας C# που διαβάζει τη ρύθμιση παραμέτρων XML από ένα αρχείο και μεταβιβάζει με την εργασία σε μια εργασία:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Προσαρμογή των ιδιοτήτων στοιχείου  

### <a name="property-with-a-simple-value"></a>Ιδιότητα με μια απλή τιμή
Σε ορισμένες περιπτώσεις, είναι χρήσιμο για να προσαρμόσετε μια ιδιότητα στοιχείο μαζί με το αρχείο ροής εργασίας που πρόκειται να εκτελεστεί από τη ροή εργασίας Premium κωδικοποιητή πολυμέσων.

Ας υποθέσουμε ότι που έχει σχεδιαστεί μια ροή εργασίας που επικαλύψεων κειμένου στο βίντεό σας και το κείμενο (για παράδειγμα, την τρέχουσα ημερομηνία) είναι πρέπει να οριστούν κατά το χρόνο εκτέλεσης. Μπορείτε να το κάνετε με το κείμενο θα οριστεί ως η νέα τιμή για την ιδιότητα κειμένου του στοιχείου επικάλυψη από την εργασία κωδικοποίησης αποστολή. Μπορείτε να χρησιμοποιήσετε αυτόν τον μηχανισμό για να αλλάξετε άλλες ιδιότητες ενός στοιχείου στη ροή εργασίας (όπως τη θέση ή το χρώμα της επικάλυψης, το ρυθμό μετάδοσης bit του κωδικοποιητή AVC, κ.λπ.).

**setRuntimeProperties** χρησιμοποιείται για να παρακάμψετε μια ιδιότητα στο τα στοιχεία της ροής εργασίας.

Παράδειγμα:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Ιδιότητα με μια τιμή XML

Για να ορίσετε μια ιδιότητα που αναμένεται μια τιμή XML, συμπύκνωση χρησιμοποιώντας `<![CDATA[ and ]]>`.

Παράδειγμα:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Βεβαιωθείτε ότι δεν για να τοποθετήσετε ένα χαρακτήρα επιστροφής μόνο αφού `<![CDATA[`.


### <a name="propertypath-value"></a>τιμή propertyPath

Στα προηγούμενα παραδείγματα, θα ήταν η propertyPath "/ αρχείων πολυμέσων εισαγωγής/όνομα_αρχείου" ή "/ inactiveTimeout" ή "clipListXml".
Αυτή είναι, σε γενικές γραμμές, το όνομα του στοιχείου, στη συνέχεια, το όνομα της ιδιότητας. Η διαδρομή μπορεί να έχει περισσότερα ή λιγότερα επίπεδα, όπως "/ primarySourceFile" (επειδή η ιδιότητα είναι στον ριζικό κατάλογο της ροής εργασίας) ή "/ βίντεο επεξεργασίας/γραφικών επικάλυψη/αδιαφάνειας" (επειδή είναι της επικάλυψης σε μια ομάδα).    

Για να ελέγξετε τη διαδρομή και την ιδιότητα το όνομα, χρησιμοποιήστε το κουμπί ενέργειας που βρίσκεται ακριβώς δίπλα σε κάθε ιδιότητα. Για να επιλέξτε αυτό το κουμπί ενέργειας και επιλέξτε **Επεξεργασία**. Αυτό θα εμφανιστεί το πραγματικό όνομα της ιδιότητας και ακριβώς επάνω από αυτό, το πεδίο ονόματος.

![Η ενέργεια/Edit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Ιδιότητα](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Πολλαπλά αρχεία εισαγωγής

Κάθε εργασίας που υποβάλετε για τη **Ροή εργασίας Premium κωδικοποιητή πολυμέσων** απαιτεί δύο στοιχεία:

- Το πρώτο είναι *Περιουσιακών στοιχείων της ροής εργασίας* που περιέχει ένα αρχείο ροής εργασίας. Μπορείτε να σχεδιάσετε αρχεία της ροής εργασίας με τη χρήση του [Προγράμματος σχεδίασης ροής εργασίας](media-services-workflow-designer.md).
- Στο δεύτερο παράδειγμα είναι μια *Διαθέσιμων στοιχείων πολυμέσων* που περιέχει τα αρχεία πολυμέσων που θέλετε να κωδικοποιήσετε.

Όταν στέλνετε πολλά αρχεία πολυμέσων για να τον κωδικοποιητή **Ροής εργασίας Premium κωδικοποιητή πολυμέσων** , ισχύουν οι παρακάτω περιορισμοί:

- Όλα τα αρχεία πολυμέσων πρέπει να είναι στο ίδιο *Διαθέσιμων στοιχείων πολυμέσων*. Χρήση πολλών πόρους πολυμέσων δεν υποστηρίζεται.
- Πρέπει να ορίσετε το αρχικό αρχείο αυτού του περιουσιακού στοιχείου πολυμέσων (ιδανικά, αυτό είναι το κύριο αρχείο βίντεο που είναι ζητείται ο κωδικοποιητής για την επεξεργασία).
- Είναι απαραίτητο για τη μεταβίβαση δεδομένων ρύθμισης παραμέτρων που περιλαμβάνει το στοιχείο **setRuntimeProperties** ή/και **transcodeSource** στον επεξεργαστή.
  - **setRuntimeProperties** χρησιμοποιείται για να παρακάμψετε την ιδιότητα filename ή άλλη ιδιότητα στα στοιχεία της ροής εργασίας.
  - **transcodeSource** χρησιμοποιείται για να καθορίσετε το περιεχόμενο XML λίστα Clip.

Οι συνδέσεις εντός της ροής εργασίας:

 - Εάν χρησιμοποιείτε ένα ή πολλά στοιχεία εισαγωγής αρχείων πολυμέσων και σχεδιασμός για χρήση **setRuntimeProperties** για να καθορίσετε το όνομα του αρχείου, στη συνέχεια, να μην γίνεται σύνδεση το pin στοιχείο πρωτεύον αρχείο σε αυτά. Βεβαιωθείτε ότι υπάρχει σύνδεση μεταξύ του αντικειμένου πρωτεύον αρχείο και το Input(s) αρχείου πολυμέσων.
 - Εάν προτιμάτε να χρησιμοποιήσετε Clip λίστα XML και ένα στοιχείο προέλευσης πολυμέσων, στη συνέχεια, μπορείτε να συνδεθείτε και τα δύο μαζί.

![Σύνδεση από το κύριο αρχείο προέλευσης με εισαγωγής αρχείων πολυμέσων](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Υπάρχει σύνδεση από το αρχικό αρχείο για να στοιχείων εισαγωγής αρχείων πολυμέσων, εάν χρησιμοποιείτε το setRuntimeProperties για να ορίσετε την ιδιότητα filename.*

![Σύνδεση από τη λίστα Clip XML στην Clip λίστας προέλευσης](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Μπορείτε να συνδεθείτε XML λίστα απόσπασμα πολυμέσων προέλευση και να χρησιμοποιήσετε transcodeSource.*


### <a name="clip-list-xml-customization"></a>Clip Προσαρμογή λίστας XML
Μπορείτε να καθορίσετε το XML λίστα Clip στη ροή εργασίας κατά το χρόνο εκτέλεσης με τη χρήση **transcodeSource** στη συμβολοσειρά ρύθμισης παραμέτρων XML. Αυτό απαιτεί το pin Clip λίστα XML για να συνδεθείτε με το στοιχείο προέλευση πολυμέσων στη ροή εργασίας.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Εάν θέλετε να καθορίσετε /primarySourceFile για να χρησιμοποιήσετε αυτήν την ιδιότητα για να ονομάσετε τα αρχεία εξόδου χρησιμοποιώντας 'Εκφράσεις', στη συνέχεια, συνιστάται να μεταβίβαση Clip λίστα XML ως μια ιδιότητα *μετά* την ιδιότητα /primarySourceFile, για να αποφύγετε τη λίστα Clip να παρακαμφθούν από τη ρύθμιση /primarySourceFile.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Με επιπλέον ακριβή πλαισίου περικοπής:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Παράδειγμα

Εξετάστε το ενδεχόμενο παράδειγμα στην οποία θέλετε να γίνει επικάλυψη μια εικόνα λογότυπου στην είσοδο βίντεο, ενώ το βίντεο κωδικοποιείται. Σε αυτό το παράδειγμα, το βίντεο εισαγωγής ονομάζεται "MyInputVideo.mp4" και το λογότυπο είναι "MyLogo.png". Θα πρέπει να εκτελέσετε τα παρακάτω βήματα:

- Δημιουργία ενός περιουσιακού στοιχείου ροής εργασίας με το αρχείο ροής εργασίας (ανατρέξτε στο ακόλουθο παράδειγμα).
- Δημιουργία ενός περιουσιακού στοιχείου πολυμέσων, το οποίο περιέχει δύο αρχεία: MyInputVideo.mp4 ως πρωτεύον αρχείο και MyLogo.png.
- Αποστολή μιας εργασίας στον επεξεργαστή πολυμέσων ροής εργασίας Premium κωδικοποιητή πολυμέσων με τα παραπάνω στοιχεία εισόδου και να καθορίσετε την ακόλουθη συμβολοσειρά ρύθμισης παραμέτρων.

Ρύθμιση παραμέτρων:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Στο παραπάνω παράδειγμα, το όνομα του αρχείου βίντεο αποστέλλεται το στοιχείο εισαγωγής αρχείου πολυμέσων και την ιδιότητα primarySourceFile. Το όνομα του αρχείου λογότυπο αποστέλλεται μια άλλη εισαγωγής αρχείων πολυμέσων που είναι συνδεδεμένη με το στοιχείο γραφικού επικάλυψης.

>[AZURE.NOTE]Το όνομα του αρχείου βίντεο θα σταλεί στην ιδιότητα primarySourceFile. Ο λόγος για αυτό είναι να χρησιμοποιήσετε αυτήν την ιδιότητα στη ροή εργασίας για τη δημιουργία του ονόματος αρχείου σωστή εξόδου χρησιμοποιώντας παραστάσεις, για παράδειγμα.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Δημιουργία βήμα προς βήμα ροής εργασίας που επικαλύψεων ένα λογότυπο επάνω από το βίντεο     

Ακολουθούν τα βήματα για να δημιουργήσετε μια ροή εργασίας που τίθεται σε δύο αρχεία ως είσοδο: βίντεο και μια εικόνα. Αυτό θα επικάλυψης την εικόνα επάνω από το βίντεο.

Ανοίξτε το **Πρόγραμμα σχεδίασης ροής εργασίας** και επιλέξτε **αρχείο** > **Νέο χώρο εργασίας** > **Μετατρέπει σχέδιο**.

Η νέα ροή εργασίας εμφανίζει τρία στοιχεία:

- Αρχείο προέλευσης πρωτεύοντος
- Λίστα clip XML
- Αρχείο εξόδου/περιουσιακών στοιχείων  

![Νέα ροή εργασίας κωδικοποίησης](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Νέα ροή εργασίας κωδικοποίησης*


Προκειμένου να αποδεχθείτε το αρχείο εισαγωγής πολυμέσων, ξεκινήστε με την προσθήκη ενός στοιχείου πολυμέσων αρχείου εισαγωγής. Για να προσθέσετε ένα στοιχείο της ροής εργασίας, αναζητήστε στο πλαίσιο αναζήτησης αποθετήριο και σύρετε την καταχώρηση που θέλετε στο παράθυρο σχεδίασης.

Στη συνέχεια, προσθέστε το αρχείο βίντεο που θα χρησιμοποιηθεί για τη Σχεδίαση ροής εργασίας. Για να το κάνετε αυτό, κάντε κλικ στην επιλογή παράθυρο φόντου στη Σχεδίαση ροής εργασίας και αναζητήστε την ιδιότητα πρωτεύον αρχείο προέλευσης στο παράθυρο ιδιοτήτων στα δεξιά. Κάντε κλικ στο εικονίδιο του φακέλου και επιλέξτε το κατάλληλο αρχείο βίντεο.

![Πρωτεύον αρχείο προέλευσης](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Πρωτεύον αρχείο προέλευσης*


Στη συνέχεια, καθορίστε το αρχείο βίντεο στο στοιχείο εισαγωγής αρχείων πολυμέσων.   

![Προέλευσης εισαγωγής αρχείων πολυμέσων](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Προέλευσης εισαγωγής αρχείων πολυμέσων*


Μόλις το κάνετε αυτό, το στοιχείο εισαγωγής αρχείων πολυμέσων θα ελέγξετε το αρχείο και να συμπληρώσετε το PIN εξόδου, προκειμένου να αντικατοπτρίζει το αρχείο που την επιθεώρηση.

Το επόμενο βήμα είναι να προσθέσετε μια "βίντεο δεδομένα τύπου προγράμματος ενημέρωσης" για να καθορίσετε το διάστημα χρώμα για να Rec.709. Προσθέστε ένα "βίντεο μετατροπέα μορφής" που έχει οριστεί σε τύπο δεδομένων διάταξη/διάταξης = με δυνατότητα ρύθμισης παραμέτρων επίπεδο. Αυτό θα μετατρέψει τη ροή βίντεο σε μια μορφή που μπορούν να ληφθούν ως προέλευση του στοιχείου επικάλυψης.

![βίντεο προγράμματος ενημέρωσης τύπος δεδομένων και μορφοποίηση μετατροπέα](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Προγράμματος ενημέρωσης τύπο δεδομένων βίντεο και μετατροπέα μορφής*

![Τύπος διάταξης = με δυνατότητα ρύθμισης παραμέτρων επίπεδο](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Τύπος διάταξης είναι δυνατή η ρύθμιση παραμέτρων επίπεδο*

Στη συνέχεια, προσθέστε ένα στοιχείο βίντεο επικάλυψης και συνδέστε το pin (μη συμπιεσμένο) βίντεο με το βίντεο (μη συμπιεσμένο) pin της εισόδου αρχείων πολυμέσων.

Προσθήκη άλλου εισαγωγής αρχείων πολυμέσων (για να φορτώσετε το αρχείο λογότυπου), κάντε κλικ σε αυτό το στοιχείο και μετονομάστε το σε "Λογότυπο εισαγωγής αρχείων πολυμέσων" και επιλέξτε μια εικόνα (ένα αρχείο .png για παράδειγμα) στην ιδιότητα αρχείου. Συνδέστε το pin αρχεία μη συμπιεσμένων εικόνων για να το pin αρχεία μη συμπιεσμένων εικόνων της επικάλυψης.

![Επικάλυψη στοιχείου και εικόνα αρχείο προέλευσης](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Επικάλυψη στοιχείου και εικόνα αρχείο προέλευσης*


Εάν θέλετε να τροποποιήσετε τη θέση του λογότυπου του βίντεο (για παράδειγμα, μπορεί να θέλετε να το τοποθετήσετε στο 10 τοις εκατό καταργήσω στην επάνω αριστερή γωνία του βίντεο), καταργήστε την επιλογή από το πλαίσιο ελέγχου "Μη αυτόματη εισαγωγή δεδομένων". Μπορείτε να το κάνετε αυτό επειδή χρησιμοποιείτε μια εισαγωγής αρχείων πολυμέσων για την παροχή το αρχείο λογότυπου για το στοιχείο επικάλυψης.

![Θέση επικάλυψη](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Θέση επικάλυψη*


Για να κωδικοποιήσετε τη ροή βίντεο για να H.264, προσθέστε τα στοιχεία κωδικοποιητή βίντεο Encoder AVC και AAC στην επιφάνεια σχεδίασης. Συνδέστε το αριθμοί PIN.
Ρύθμιση ο κωδικοποιητής AAC και επιλέξτε μορφή ήχου μετατροπής/υπόδειγμα: 2.0 (L, R).

![Κωδικοποιητές ήχου και βίντεο](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Κωδικοποιητές ήχου και βίντεο*


Τώρα, προσθέστε τα στοιχεία **ISO Mpeg-4 Πολυπλέκτης** και **Το αποτέλεσμα του αρχείου** και σύνδεση το αριθμοί PIN, όπως φαίνεται.

![Πολυπλέκτης mp4 και το αποτέλεσμα του αρχείου](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*Πολυπλέκτης mp4 και το αποτέλεσμα του αρχείου*


Πρέπει να ορίσετε το όνομα για το αρχείο εξόδου. Κάντε κλικ στο στοιχείο **Αρχείο εξόδου** και επεξεργαστείτε την παράσταση για το αρχείο:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Όνομα αρχείου εξόδου](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Όνομα αρχείου εξόδου*

Μπορείτε να εκτελέσετε τη ροή εργασίας τοπικά για να ελέγξετε ότι λειτουργεί σωστά.

Μετά την ολοκλήρωση της, μπορείτε να το εκτελέσετε στο υπηρεσιών Azure Media Services.

Πρώτα, προετοιμασία ενός περιουσιακού στοιχείου στις υπηρεσίες πολυμέσων Azure με δύο αρχεία σε αυτό: το αρχείο βίντεο και το λογότυπο. Μπορείτε να το κάνετε χρησιμοποιώντας το .NET ή REST API. Μπορείτε επίσης να κάνετε αυτό, χρησιμοποιώντας το Azure πύλη ή στην [Εξερεύνηση των υπηρεσιών Azure Media](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να διαχειριστείτε πόρους με AMSE. Υπάρχουν δύο τρόποι για να προσθέσετε αρχεία στο ενός περιουσιακού στοιχείου:

- Δημιουργήστε ένα τοπικό φάκελο, αντιγράψτε τα δύο αρχεία του, και σύρετε και αποθέστε το φάκελο στην καρτέλα **περιουσιακών στοιχείων** .
- Αποστείλετε το αρχείο βίντεο ως ενός περιουσιακού στοιχείου, εμφάνιση της πληροφορίας περιουσιακού στοιχείου, μεταβείτε στην καρτέλα αρχεία και αποστολή αρχείου επιπλέον (λογότυπο).

>[AZURE.NOTE]Βεβαιωθείτε ότι για να ορίσετε ένα πρωτεύον αρχείο παγίου (το κύριο αρχείο βίντεο).

![Αρχεία περιουσιακών στοιχείων στο AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Αρχεία περιουσιακών στοιχείων στο AMSE*


Επιλέξτε τον πόρο και επιλέξτε για την κωδικοποίηση με Premium Encoder. Αποστολή της ροής εργασίας και επιλέξτε την.

Κάντε κλικ στο κουμπί για τη μεταβίβαση δεδομένων στον επεξεργαστή και προσθέστε το παρακάτω δείγμα XML για να ορίσετε τις ιδιότητες του χρόνου εκτέλεσης:

![Premium Encoder στο AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium Encoder στο AMSE*


Στη συνέχεια, επικολλήστε τα ακόλουθα δεδομένα XML. Πρέπει να καθορίσετε το όνομα του αρχείου βίντεο τόσο για την εισαγωγή αρχείου πολυμέσων και primarySourceFile. Καθορίστε το όνομα του ονόματος αρχείου για το λογότυπο πολύ.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Εάν χρησιμοποιείτε το .NET SDK για να δημιουργήσετε και να εκτελέσετε την εργασία, αυτά τα δεδομένα XML έχει θα μεταβιβάζεται ως η συμβολοσειρά ρύθμισης παραμέτρων.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Όταν ολοκληρωθεί η εργασία, το αρχείο MP4 στο παγίου εξόδου εμφανίζει την επικάλυψη!

![Επικάλυψης σε βίντεο](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Επικάλυψης σε βίντεο*


Μπορείτε να κάνετε λήψη της ροής εργασίας δείγμα από [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Δείτε επίσης

- [Εισαγωγή Premium κωδικοποίηση στο Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Πώς μπορείτε να χρησιμοποιήσετε κωδικοποίηση Premium στις υπηρεσίες πολυμέσων Azure](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Κωδικοποίηση περιεχομένου σε ζήτηση με τις υπηρεσίες πολυμέσων Azure](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Μορφές ροής εργασίας Premium κωδικοποιητή πολυμέσων και τους κωδικοποιητές](media-services-premium-workflow-encoder-formats.md)

- [Δείγματα αρχείων ροής εργασίας](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Εργαλείο Explorer υπηρεσίες πολυμέσων Azure](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
