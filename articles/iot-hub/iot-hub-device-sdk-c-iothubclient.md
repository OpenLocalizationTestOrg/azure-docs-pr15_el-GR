<properties
    pageTitle="Azure IoT συσκευή SDK για C - IoTHubClient | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τη χρήση της βιβλιοθήκης IoTHubClient στη συσκευή Azure IoT SDK για C"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT συσκευή SDK για C-περισσότερες πληροφορίες σχετικά με IoTHubClient

Το [πρώτο άρθρο](iot-hub-device-sdk-c-intro.md) σε αυτήν τη σειρά εισάγονται στη **συσκευή Microsoft Azure IoT SDK για C**. Αυτό το άρθρο επεξήγηση ότι υπάρχουν δύο επίπεδα αρχιτεκτονικής στο SDK. Στη βάση είναι η βιβλιοθήκη **IoTHubClient** που διαχειρίζεται απευθείας την επικοινωνία με το Κέντρο IoT. Υπάρχει επίσης στη βιβλιοθήκη **σειριοποιητής** που δημιουργεί πάνω σε αυτό για να παρέχουν υπηρεσίες σειριοποίησης. Σε αυτό το άρθρο θα σας θα παρέχουν πρόσθετες λεπτομέρειες σχετικά με τη βιβλιοθήκη **IoTHubClient** .

Το προηγούμενο άρθρο περιγράφεται πώς μπορείτε να χρησιμοποιήσετε τη βιβλιοθήκη **IoTHubClient** για να στείλετε συμβάντα IoT διανομέας και να λαμβάνετε μηνύματα. Σε αυτό το άρθρο επεκτείνει αυτήν τη συζήτηση εξηγώντας τον τρόπο διαχείρισης μεγαλύτερη ακρίβεια *Όταν* στέλνετε και λαμβάνετε δεδομένα, εισαγωγή μπορείτε να τα **APIs κατώτερου επιπέδου**. Θα σας επίσης θα εξηγούν τον τρόπο για να επισυνάψετε ιδιότητες σε συμβάντα (και να τις ανακτήσετε από τα μηνύματα) χρησιμοποιώντας την ιδιότητα χειρισμός δυνατότητες στη βιβλιοθήκη **IoTHubClient** . Τέλος, θα παρέχουμε επιπλέον εξήγηση των διαφορετικούς τρόπους χειρισμού των μηνυμάτων που έχουν ληφθεί από διανομέα IoT.

Το άρθρο συμπεραίνει με που καλύπτεται από δύο διάφορα θέματα, συμπεριλαμβανομένων των περισσότερα σχετικά με τη συσκευή διαπιστευτήρια και πώς μπορείτε να αλλάξετε τη συμπεριφορά του **IoTHubClient** στις επιλογές ρύθμισης παραμέτρων.

Θα χρησιμοποιήσουμε τα δείγματα SDK **IoTHubClient** για να εξηγήσετε αυτά τα θέματα. Εάν θέλετε να παρακολουθήσετε κατά μήκος, ανατρέξτε στο θέμα το **iothub\_προγράμματος-πελάτη\_δείγμα\_http** και **iothub\_προγράμματος-πελάτη\_δείγμα\_amqp** εφαρμογές που περιλαμβάνονται στη συσκευή Azure IoT SDK για C. όλα τα στοιχεία που περιγράφεται στις παρακάτω ενότητες περιγράφονται στις αυτά τα δείγματα.

Μπορείτε να βρείτε τη **συσκευή Azure IoT SDK για C** στο αποθετήριο δεδομένων [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) GitHub και να προβάλετε λεπτομέρειες του API της [αναφοράς C API](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Τα API κατώτερου επιπέδου

Το προηγούμενο άρθρο περιγράφεται η βασική λειτουργία του το **IotHubClient** στο περιβάλλον του το **iothub\_προγράμματος-πελάτη\_δείγμα\_amqp** εφαρμογής. Για παράδειγμα, εξηγείται πώς να προετοιμάσετε στη βιβλιοθήκη με αυτόν τον κωδικό.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Το περιγράφεται επίσης πώς μπορείτε να στείλετε συμβάντα χρησιμοποιώντας αυτή η κλήση συνάρτησης.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Το άρθρο περιγράφεται επίσης πώς μπορείτε να λαμβάνετε μηνύματα με την καταχώρηση μιας συνάρτησης επιστροφής κλήσης.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Το άρθρο εμφάνιζε επίσης τον τρόπο για να αποδεσμεύσετε πόρους που χρησιμοποιεί κώδικα όπως οι εξής.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Ωστόσο, υπάρχουν συνοδευτικό συναρτήσεις σε καθένα από αυτά τα API:

-   IoTHubClient\_α\_CreateFromConnectionString

-   IoTHubClient\_α\_SendEventAsync

-   IoTHubClient\_α\_SetMessageCallback

-   IoTHubClient\_α\_καταστροφή


Αυτές οι συναρτήσεις όλα περιλαμβάνουν "Α" στο όνομα του API. Εκτός από αυτό, οι παράμετροι του κάθε μία από αυτές τις συναρτήσεις είναι ίδιες με τις αντίστοιχες εκδόσεις για μη α. Ωστόσο, η συμπεριφορά από αυτές τις συναρτήσεις είναι διαφορετικό σε ένα σημαντικό τρόπο.

Όταν καλείτε **IoTHubClient\_CreateFromConnectionString**, τα υποκείμενα βιβλιοθήκες, δημιουργήστε ένα νέο νήμα που εκτελείται στο παρασκήνιο. Αυτού του νήματος συμβάντα που θέλετε να στέλνει και λαμβάνει μηνύματα από το Κέντρο IoT. Δεν υπάρχει όπως νήματος δημιουργείται κατά την εργασία με το "α" APIs. Η δημιουργία του νήματος φόντου είναι εξυπηρέτηση στον προγραμματιστή. Δεν χρειάζεται να ανησυχείτε για ρητά συμβάντα αποστολή και λήψη μηνυμάτων από διανομέα IoT--γίνεται αυτόματα στο παρασκήνιο. Αντίθετα, το "α" APIs δώσει ρητή έλεγχο επικοινωνίας με το διανομέα IoT, εάν τον χρειάζεστε.

Για να κατανοήσετε καλύτερα αυτό, ας δούμε ένα παράδειγμα:

Όταν καλείτε **IoTHubClient\_SendEventAsync**, τι πραγματικά κάνετε τοποθετεί το συμβάν σε buffer. Το φόντο νήμα δημιουργήθηκε όταν καλείτε **IoTHubClient\_CreateFromConnectionString** συνεχώς παρακολουθεί αυτό το buffer και στέλνει τα δεδομένα που περιέχει IoT διανομέα. Αυτό συμβαίνει στο παρασκήνιο την ίδια στιγμή, ότι το κύριο νήμα εκτελεί άλλη εργασία.

Ομοίως, όταν καταχωρείτε μιας συνάρτησης επιστροφής κλήσης για μηνύματα χρησιμοποιώντας το **IoTHubClient\_SetMessageCallback**, καθοδηγώντας το SDK για να έχετε το φόντο νήμα κλήση της συνάρτησης επιστροφής κλήσης όταν ένα μήνυμα παραλήφθηκε, ανεξάρτητα από το κύριο νήμα.

Το "α" APIs δεν δημιουργήσετε ένα νήμα παρασκηνίου. Αντί για αυτό, πρέπει να καλείται μια νέα API για ρητά αποστολή και λήψη δεδομένων από διανομέα IoT. Αυτή η περίπτωση περιγράφεται στο παρακάτω παράδειγμα.

Το **iothub\_προγράμματος-πελάτη\_δείγμα\_http** εφαρμογής που περιλαμβάνεται στο SDK παρουσιάζει τα API κατώτερου επιπέδου. Σε αυτό το δείγμα, στείλουμε συμβάντα IoT διανομέα με κώδικα, όπως οι εξής:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Οι τρεις πρώτες γραμμές, δημιουργήστε το μήνυμα και την τελευταία γραμμή στέλνει το συμβάν. Ωστόσο, όπως προαναφέρθηκε, "Αποστολή" στο συμβάν σημαίνει ότι τα δεδομένα είναι απλώς τοποθετούνται σε buffer. Τίποτα μεταδίδεται στο δίκτυο, όταν ονομάζουμε **IoTHubClient\_α\_SendEventAsync**. Προκειμένου να εισόδου στην πραγματικότητα τα δεδομένα με IoT διανομέα, πρέπει να καλέσετε **IoTHubClient\_α\_DoWork**, καθώς και σε αυτό το παράδειγμα:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Αυτός ο κωδικός (από το **iothub\_προγράμματος-πελάτη\_δείγμα\_http** εφαρμογής) επανειλημμένα κλήσεις **IoTHubClient\_α\_DoWork**. Κάθε φορά που **IoTHubClient\_α\_DoWork** είναι ονομάζεται, στέλνει ορισμένα συμβάντα από το buffer με διανομέα IoT και ανακτά ένα μήνυμα σε ουρά που στέλνεται στη συσκευή. Τελευταία περίπτωση σημαίνει ότι αν θα σας καταχωρήσει μιας συνάρτησης επιστροφής κλήσης για τα μηνύματα, στη συνέχεια, την επιστροφή κλήσης ενεργοποιείται (αν υποθέσουμε ότι όλα τα μηνύματα έχουν τοποθετηθεί στην ουρά). Θα σας θα έχετε καταχωρήσει μιας συνάρτησης επιστροφής κλήσης με κώδικα, όπως οι εξής:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Ο λόγος που **IoTHubClient\_α\_DoWork** ονομάζεται συχνά σε επανάληψη είναι ότι κάθε φορά που ονομάζεται, αποστέλλει *ορισμένες* σε buffer συμβάντα IoT διανομέας και ανακτά *την επόμενη* μηνύματος στην ουρά για τη συσκευή. Κάθε κλήση δεν εγγυάται για να στείλετε όλα buffered συμβάντα ή για την ανάκτηση όλων στην ουρά μηνυμάτων. Εάν θέλετε να στείλετε όλα τα συμβάντα στο buffer και, στη συνέχεια, συνεχίστε με άλλη επεξεργασία μπορείτε να αντικαταστήσετε αυτό βρόχος με κώδικα όπως οι εξής:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Αυτός ο κώδικας καλεί **IoTHubClient\_α\_DoWork** μέχρι να έχουν σταλεί όλα τα συμβάντα στο buffer με IoT διανομέα. Σημείωση Αυτή δεν επίσης σημαίνει ότι έχουν ληφθεί όλα τα μηνύματα σε ουρά. Μέρος της ο λόγος είναι ότι τον έλεγχο για μηνύματα "όλα" δεν είναι ντετερμινιστική ως μια ενέργεια. Τι θα συμβεί εάν κάνετε ανάκτηση "όλα" των μηνυμάτων, αλλά, στη συνέχεια, αποστέλλεται μια άλλη συσκευή αμέσως μετά την; Καλύτερος τρόπος για να ασχοληθείτε με αυτό είναι ένα προγραμματισμένο χρονικό όριο. Για παράδειγμα, η συνάρτηση επιστροφής κλήσης μήνυμα ήταν δυνατή η επαναφορά ενός χρονομέτρου κάθε φορά που ενεργοποιείται. Στη συνέχεια, μπορείτε να συντάξετε λογικής για να συνεχίσετε την επεξεργασία εάν, για παράδειγμα, τα μηνύματα δεν έχουν ληφθεί τα τελευταία *X* δευτερόλεπτα.

Όταν είστε έτοιμοι ingressing συμβάντα και λαμβάνετε μηνύματα, φροντίστε να καλέσετε το αντίστοιχο συνάρτηση για να καθαρίσετε πόρους.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Στην ουσία υπάρχει μόνο ένα σύνολο API για την αποστολή και λήψη δεδομένων με ένα νήμα παρασκηνίου και ένα άλλο σύνολο API που κάνει το ίδιο χωρίς το νήμα φόντου. Πολλές προγραμματιστές ίσως προτιμάτε τα μη α API, αλλά τα API κατώτερου επιπέδου είναι χρήσιμα όταν ο προγραμματιστής θέλει ρητή ελέγχετε περισσότερο τη μετάδοση δικτύου. Για παράδειγμα, ορισμένες συσκευές συλλογή δεδομένων μέσα στο χρόνο και συμβάντα μόνο εισόδου σε καθορισμένα χρονικά διαστήματα (για παράδειγμα, μία φορά μια ώρα ή μία φορά την ημέρα). Τα API κατώτερου επιπέδου σας δίνουν τη δυνατότητα να ρητά ελέγχου κατά την αποστολή και λήψη δεδομένων από διανομέα IoT. Άλλοι απλώς θα προτιμάτε την απλότητα που παρέχουν τα API κατώτερου επιπέδου. Όλα τα στοιχεία θα συμβεί σε κύριο νήμα αντί για ορισμένες λειτουργία εργασίας στο παρασκήνιο.

Ανάλογα με το μοντέλο επιλέξετε, φροντίστε να είναι συνεπείς στα οποία μπορείτε να χρησιμοποιήσετε API. Εάν ξεκινήσετε καλώντας **IoTHubClient\_α\_CreateFromConnectionString**, βεβαιωθείτε ότι χρησιμοποιείτε μόνο τα αντίστοιχα API κατώτερου επιπέδου για οποιαδήποτε εργασία παρακολούθησης:

-   IoTHubClient\_α\_SendEventAsync

-   IoTHubClient\_α\_SetMessageCallback

-   IoTHubClient\_α\_καταστροφή

-   IoTHubClient\_α\_DoWork

Το αντίθετο είναι αληθής, καθώς και. Εάν ξεκινήσετε με **IoTHubClient\_CreateFromConnectionString**, στη συνέχεια, χρησιμοποιήστε τα μη α API για κάθε επιπλέον επεξεργασίας.

Στο τη συσκευή Azure IoT SDK για C, ανατρέξτε στο θέμα το **iothub\_προγράμματος-πελάτη\_δείγμα\_http** εφαρμογής για μια πλήρη παράδειγμα των API του κατώτερου επιπέδου. Το **iothub\_προγράμματος-πελάτη\_δείγμα\_amqp** εφαρμογής μπορούν να χρησιμοποιηθούν για μια πλήρη παράδειγμα από τα μη - α API.

## <a name="property-handling"></a>Ιδιότητα χειρισμού

Μέχρι στιγμής όταν θα σας έχετε περιγράφεται αποστολή δεδομένων, θα σας έχετε που αναφέρονται στο κυρίως σώμα του μηνύματος. Για παράδειγμα, εξετάστε το ενδεχόμενο να αυτόν τον κώδικα:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Αυτό το παράδειγμα στέλνει ένα μήνυμα σε διανομέα IoT με το κείμενο "Χαίρετε". Ωστόσο, διανομέα IoT επιτρέπει επίσης ιδιότητες, για να επισυναφθεί σε κάθε μήνυμα. Οι ιδιότητες είναι ζεύγη ονόματος/τιμής που μπορείτε να το επισυνάψετε στο μήνυμα. Για παράδειγμα, θα σας να τροποποιήσετε τον προηγούμενο κώδικα για να επισυνάψετε μια ιδιότητα στο μήνυμα:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Θα σας Έναρξη καλώντας **IoTHubMessage\_ιδιότητες** και διοχέτευση τη λαβή μας μηνύματος. Τι θα σας να επιστρέψετε είναι μια **ΧΆΡΤΗ\_ΧΕΙΡΙΣΜΟΎ** αναφοράς που σας επιτρέπει να ξεκινήσετε να προσθέτετε ιδιότητες. Η τελευταία πραγματοποιείται καλώντας **Χάρτης\_AddOrUpdate**, που θα λαμβάνει μια αναφορά σε ένα ΧΆΡΤΗ\_ΛΑΒΉ, το όνομα της ιδιότητας και την τιμή της ιδιότητας. Με αυτό το API μπορεί να προσθέσουμε όσο το δυνατόν περισσότερες ιδιότητες όπως θα σας αρέσει.

Κατά την ανάγνωση το συμβάν από το **Συμβάν διανομείς**, ο παραλήπτης να απαρίθμηση των ιδιοτήτων και να ανακτήσετε τις αντίστοιχες τιμές. Για παράδειγμα, στο .NET αυτό μπορεί να πραγματοποιηθεί από την πρόσβαση στη [συλλογή Properties του αντικειμένου EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Στο προηγούμενο παράδειγμα, θα σας επισύναψη ιδιότητες σε ένα συμβάν που θα σας στείλουμε IoT διανομέα. Ιδιότητες μπορείτε επίσης να το επισυνάψετε μηνυμάτων που έχουν ληφθεί από διανομέα IoT. Εάν θέλουμε να ανακτήσετε ιδιότητες από ένα μήνυμα, μπορούμε να χρησιμοποιήσουμε κώδικα όπως τα ακόλουθα στο μας λειτουργία επιστροφής κλήσης μήνυμα:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Την κλήση σε **IoTHubMessage\_ιδιότητες** επιστρέφει την **ΧΆΡΤΗ\_ΧΕΙΡΙΣΜΟΎ** αναφοράς. Θα σας, στη συνέχεια, μεταβιβάζουν συγκεκριμένη αναφορά σε **χάρτη\_GetInternals** για να αποκτήσετε μια αναφορά σε έναν πίνακα με τα ζεύγη ονόματος/τιμής (καθώς και μια καταμέτρηση των ιδιοτήτων). Σε αυτό το σημείο είναι ένα απλό ζήτημα απαρίθμηση των ιδιοτήτων για να μεταβείτε στις τιμές που θέλουμε.

Δεν χρειάζεται να χρησιμοποιήσετε ιδιότητες στην εφαρμογή σας. Ωστόσο, εάν χρειάζεστε για να ορίσετε τα συμβάντα ή να τις ανακτήσετε από τα μηνύματα, τη βιβλιοθήκη **IoTHubClient** καθιστά εύκολη.

## <a name="message-handling"></a>Χειρισμό μηνυμάτων

Όπως αναφέρθηκε προηγουμένως, κατά την άφιξη μηνυμάτων από διανομέα IoT στη βιβλιοθήκη **IoTHubClient** ανταποκρίνεται καλώντας μιας συνάρτησης καταχωρημένες επιστροφής κλήσης. Υπάρχει μια παράμετρο επιστροφής αυτής της συνάρτησης που αξίζει ορισμένες επιπλέον επεξήγηση. Ακολουθεί ένα απόσπασμα της συνάρτησης επιστροφής κλήσης στο το **iothub\_προγράμματος-πελάτη\_δείγμα\_http** δείγμα εφαρμογής:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Σημειώστε ότι ο επιστρεφόμενος τύπος είναι **IOTHUBMESSAGE\_ΚΑΤΑΣΤΡΟΦΉΣ\_ΑΠΟΤΈΛΕΣΜΑ** και στη συγκεκριμένη περίπτωση θα σας επιστροφής **IOTHUBMESSAGE\_ΑΠΟΔΕΚΤΟΊ**. Υπάρχουν άλλες τιμές που θα σας να επιστρέψετε από αυτήν τη συνάρτηση που αλλάζει τον τρόπο στη βιβλιοθήκη **IoTHubClient** απαντά η επιστροφή κλήσης μήνυμα. Εδώ θα βρείτε τις επιλογές.

-   **IOTHUBMESSAGE\_ΑΠΟΔΕΚΤΟΊ** – το μήνυμα έχει υποβληθεί σε επεξεργασία με επιτυχία. Η βιβλιοθήκη **IoTHubClient** δεν θα ενεργοποιήσει τη συνάρτηση επιστροφής κλήσης ξανά με το ίδιο μήνυμα.

-   **IOTHUBMESSAGE\_ΑΠΟΡΡΊΦΘΗΚΕ** -δεν έγινε επεξεργασία του μηνύματος και δεν υπάρχει καμία το ενδιαφέρον για να το κάνετε στο μέλλον. Στη βιβλιοθήκη **IoTHubClient** δεν θα πρέπει να καλέσετε τη συνάρτηση επιστροφής κλήσης ξανά με το ίδιο μήνυμα.

-   **IOTHUBMESSAGE\_ABANDONED** – το μήνυμα δεν έγινε επεξεργασία με επιτυχία, αλλά η βιβλιοθήκη **IoTHubClient** θα πρέπει να καλέσετε τη συνάρτηση επιστροφής κλήσης ξανά με το ίδιο μήνυμα.

Για τα πρώτα δύο κωδικούς επιστροφής, τη βιβλιοθήκη **IoTHubClient** στέλνει ένα μήνυμα με IoT διανομέα που υποδεικνύει ότι το μήνυμα πρέπει να διαγραφεί από την ουρά συσκευής και δεν παραδόθηκε ξανά. Το καθαρό αποτέλεσμα είναι η ίδια (το μήνυμα διαγράφεται και από τη συσκευή ουρά), αλλά εάν το μήνυμα έχει αποδεχτεί ή απορρίψει εξακολουθεί να έχει εγγραφεί.  Η εγγραφή αυτή η διάκριση είναι χρήσιμη σε αποστολείς του μηνύματος που μπορεί να ακούσετε σχόλια και να μάθετε εάν η συσκευή διαθέτει αποδοχή ή απόρριψη ένα συγκεκριμένο μήνυμα.

Στην τελευταία περίπτωση επίσης την αποστολή ενός μηνύματος με IoT διανομέα, αλλά αυτό υποδεικνύει ότι το μήνυμα θα πρέπει να είναι redelivered. Συνήθως να θα εγκαταλείψετε ένα μήνυμα Εάν αντιμετωπίσετε κάποιο σφάλμα, αλλά θέλετε να δοκιμάσετε να επεξεργαστεί ξανά το μήνυμα. Αντίθετα, η απόρριψη ένα μήνυμα είναι κατάλληλη όταν αντιμετωπίζετε ένα σφάλμα ανακτηθούν (ή αν μπορείτε απλώς να αποφασίσετε που δεν θέλετε να επεξεργαστείτε το μήνυμα).

Σε κάθε περίπτωση, λάβετε υπόψη τους διαφορετικούς κωδικούς επιστροφής, έτσι ώστε να προκαλούν τη συμπεριφορά που θέλετε από τη βιβλιοθήκη **IoTHubClient** .

## <a name="alternate-device-credentials"></a>Τα διαπιστευτήρια εναλλακτική συσκευή

Όπως εξηγείται προηγουμένως, το πρώτο πράγμα που πρέπει να κάνετε όταν εργάζεστε με τη βιβλιοθήκη **IoTHubClient** είναι για να αποκτήσετε μια **IOTHUB\_προγράμματος-ΠΕΛΆΤΗ\_ΧΕΙΡΙΣΜΟΎ** με μια κλήση, όπως τα εξής:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Τα ορίσματα της συνάρτησης **IoTHubClient\_CreateFromConnectionString** είναι η συμβολοσειρά σύνδεσης της παραμέτρου που υποδεικνύει το πρωτόκολλο που χρησιμοποιούμε για να επικοινωνήσετε με το Κέντρο IoT και μας συσκευής. Η συμβολοσειρά σύνδεσης περιλαμβάνει μια μορφή που εμφανίζεται ως εξής:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Υπάρχουν τέσσερα τμήματα των πληροφοριών σε αυτήν τη συμβολοσειρά: όνομα IoT διανομέα, διανομέα IoT επίθημα, Αναγνωριστικό συσκευής και κλειδί κοινόχρηστη πρόσβαση. Αποκτήστε το πλήρως προσδιορισμένο όνομα τομέα (FQDN) με ένα διανομέα IoT όταν δημιουργείτε την περίοδο λειτουργίας διανομέα IoT στην πύλη του Azure — το αποτέλεσμα είναι το όνομα διανομέας IoT (το πρώτο μέρος από το FQDN) και το επίθημα διανομέα IoT (τα υπόλοιπα το FQDN). Λαμβάνετε το Αναγνωριστικό συσκευής και το πλήκτρο πρόσβασης σε κοινή χρήση όταν καταχωρείτε τη συσκευή σας με το διανομέα IoT (όπως περιγράφεται στο [προηγούμενο άρθρο](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** παρέχει έναν τρόπο για την προετοιμασία της βιβλιοθήκης. Εάν προτιμάτε, μπορείτε να δημιουργήσετε μια νέα **IOTHUB\_προγράμματος-ΠΕΛΆΤΗ\_ΧΕΙΡΙΣΜΟΎ** με τη χρήση αυτών των μεμονωμένων παραμέτρους αντί για τη συμβολοσειρά σύνδεσης. Αυτό είναι δυνατό με τον ακόλουθο κώδικα:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Αυτό επιτυγχάνει το ίδιο με **IoTHubClient\_CreateFromConnectionString**.

Αυτό μπορεί να φαίνεται εμφανή ότι θα θέλετε να χρησιμοποιήσετε **IoTHubClient\_CreateFromConnectionString** και όχι αυτή τη μέθοδο πιο λεπτομερή προετοιμασίας. Έχετε υπόψη, ωστόσο, ότι όταν καταχωρείτε μια συσκευή σε διανομέα IoT τι μπορείτε να είναι ένα Αναγνωριστικό συσκευής και κλειδί συσκευής (όχι μια συμβολοσειρά σύνδεσης). Το εργαλείο SDK **Διαχείριση συσκευών** που έχουν εισαχθεί στο [προηγούμενο άρθρο](iot-hub-device-sdk-c-intro.md) χρησιμοποιεί βιβλιοθήκες στην **υπηρεσία Azure IoT SDK** για να δημιουργήσετε τη συμβολοσειρά σύνδεσης από το Αναγνωριστικό συσκευής, το κλειδί συσκευής και διανομέα IoT όνομα κεντρικού υπολογιστή. Επομένως, κλήση **IoTHubClient\_α\_δημιουργία** μπορεί να είναι προτιμότερη επειδή που αποθηκεύει το βήμα της δημιουργίας μια συμβολοσειρά σύνδεσης. Χρησιμοποιήστε όποια μέθοδο είναι εύκολη.

## <a name="configuration-options"></a>Επιλογές ρύθμισης παραμέτρων

Μέχρι στιγμής όλα τα στοιχεία που περιγράφονται σχετικά με τον τρόπο που λειτουργεί η βιβλιοθήκη **IoTHubClient** απεικονίζει την προεπιλεγμένη συμπεριφορά. Ωστόσο, υπάρχουν μερικές επιλογές που μπορείτε να ρυθμίσετε για να αλλάξετε τον τρόπο λειτουργίας της βιβλιοθήκης. Αυτή η ενέργεια πραγματοποιείται από αξιοποίηση του **IoTHubClient\_α\_SetOption** API. Εξετάστε το ενδεχόμενο να αυτό το παράδειγμα:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Υπάρχουν δύο επιλογές που χρησιμοποιούνται συνήθως:

-   **SetBatching** (δυαδικός) – Εάν **ισχύει**, στη συνέχεια, τα δεδομένα που αποστέλλονται σε διανομέα IoT αποστέλλεται σε δέσμες. Εάν η **τιμή false**, στη συνέχεια, τα μηνύματα αποστέλλονται ξεχωριστά. Η προεπιλογή είναι **false**. Σημειώστε ότι η επιλογή **SetBatching** ισχύει μόνο για το πρωτόκολλο HTTP και όχι για τα πρωτόκολλα MQTT ή AMQP.

-   **Λήξη χρονικού ορίου** (unsigned int) – Αυτή η τιμή αντιπροσωπεύεται σε χιλιοστά του δευτερολέπτου. Εάν στείλετε μια αίτηση HTTP ή να λάβετε μια απάντηση διαρκεί περισσότερο από αυτήν τη φορά και, στη συνέχεια, το χρονικό όριο σύνδεσης.

Η επιλογή διαδικασίας δέσμης είναι σημαντικές. Από προεπιλογή, τα συμβάντα ingresses βιβλιοθήκη μεμονωμένα (ένα μεμονωμένο συμβάν είναι ό, τι έχετε στείλει **IoTHubClient\_α\_SendEventAsync**). Εάν η επιλογή διαδικασίας δέσμης είναι **true**, η βιβλιοθήκη συλλέγει όσες συμβάντα μπορεί από το buffer (έως και το μέγιστο μέγεθος μηνύματος που θα δέχονται IoT διανομέα).  Η δέσμη συμβάν αποστέλλεται IoT διανομέα σε μία μόνο κλήση HTTP (τα μεμονωμένα συμβάντα βρίσκονται μέσα σε έναν πίνακα JSON). Ενεργοποίηση δέσμης για συνήθως αποτέλεσμα κέρδη μεγάλο απόδοσης δεδομένου ότι μείωση απαιτείται επικοινωνία με το δίκτυο. Μειώνει επίσης σημαντικά εύρους ζώνης, επειδή το στέλνετε ένα σύνολο των κεφαλίδων HTTP με μια δέσμη συμβάντων και όχι ένα σύνολο κεφαλίδων για κάθε μεμονωμένο συμβάν. Εάν δεν έχετε ένα συγκεκριμένο λόγο για να το κάνετε διαφορετικά, συνήθως θα θέλετε να Ενεργοποίηση δέσμης.

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο περιγράφει με λεπτομέρεια τη συμπεριφορά της βιβλιοθήκης **IoTHubClient** βρίσκονται στη **συσκευή Azure IoT SDK για C**. Με αυτές τις πληροφορίες, θα πρέπει να έχετε κατανοήσει καλά τις δυνατότητες της βιβλιοθήκης **IoTHubClient** . Το [Επόμενο άρθρο](iot-hub-device-sdk-c-serializer.md) παρέχει παρόμοια λεπτομερειών σχετικά με τη βιβλιοθήκη **σειριοποιητής** .

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη IoT διανομέα, ανατρέξτε στο θέμα το [SDK διανομέα IoT][lnk-sdks].

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md