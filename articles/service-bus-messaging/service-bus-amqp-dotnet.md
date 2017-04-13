<properties 
    pageTitle="Λειτουργία Bus με .NET και AMQP 1.0 | Microsoft Azure"
    description="Χρήση υπηρεσίας Bus από .NET με AMQP"
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Χρήση υπηρεσίας Bus από .NET με AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Λήψη του Bus υπηρεσίας SDK

Υποστήριξη AMQP 1.0 είναι διαθέσιμη στην υπηρεσία Bus SDK έκδοση 2.1 ή νεότερη έκδοση. Μπορείτε να διασφαλίσετε έχετε την πιο πρόσφατη έκδοση κάνοντας λήψη τα bit Bus υπηρεσίας από [NuGet][].

## <a name="configuring-net-applications-to-use-amqp-10"></a>Ρύθμιση παραμέτρων των εφαρμογών .NET για να χρησιμοποιήσετε AMQP 1.0

Από προεπιλογή, τη βιβλιοθήκη προγράμματος-πελάτη υπηρεσίας Bus .NET επικοινωνεί με την υπηρεσία Bus υπηρεσία χρησιμοποιώντας ένα αποκλειστικό πρωτόκολλο SOAP βάσει. Για να χρησιμοποιήσετε AMQP 1.0 αντί για την προεπιλεγμένη πρωτόκολλο απαιτεί ρητή ρύθμισης παραμέτρων στη συμβολοσειρά σύνδεσης Bus υπηρεσία, όπως περιγράφεται στην επόμενη ενότητα. Εκτός από αυτήν την αλλαγή, κώδικα της εφαρμογής παραμένει αμετάβλητη κατά τη χρήση AMQP 1.0.

Στην τρέχουσα έκδοση, υπάρχουν μερικές δυνατότητες API που δεν υποστηρίζονται κατά τη χρήση AMQP. Αυτές οι δυνατότητες που δεν υποστηρίζονται παρατίθενται αργότερα στην ενότητα [μη υποστηριζόμενες δυνατότητες, περιορισμούς, και συμπεριφοράς διαφορές](#unsupported-features-restrictions-and-behavioral-differences). Ορισμένες από τις ρυθμίσεις για προχωρημένους παραμέτρων έχουν διαφορετική έννοια επίσης κατά τη χρήση AMQP.

### <a name="configuration-using-appconfig"></a>Ρύθμιση παραμέτρων χρησιμοποιώντας App.config

Είναι καλή πρακτική για τις εφαρμογές για να χρησιμοποιήσετε το αρχείο ρύθμισης παραμέτρων App.config για την αποθήκευση ρυθμίσεων. Για τις εφαρμογές υπηρεσίας Bus, μπορείτε να χρησιμοποιήσετε App.config για να αποθηκεύσετε τις ρυθμίσεις για την τιμή υπηρεσία Bus **συμβολοσειρά σύνδεσης** . Ένα παράδειγμα αρχείου App.config είναι ως εξής:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Η τιμή του το `Microsoft.ServiceBus.ConnectionString` η ρύθμιση είναι συμβολοσειρά σύνδεσης υπηρεσίας Bus που χρησιμοποιείται για τη ρύθμιση παραμέτρων της σύνδεσης με την υπηρεσία Bus. Η μορφή είναι ως εξής:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Όπου `[namespace]` και `SharedAccessKey` λαμβάνονται από την [πύλη του Azure][]. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το ουρές Bus υπηρεσίας][].

Όταν χρησιμοποιείτε AMQP, προσαρτήσετε τη συμβολοσειρά σύνδεσης με `;TransportType=Amqp`. Αυτή η notation ενημερώνει για τη βιβλιοθήκη προγράμματος-πελάτη για να κάνετε τη σύνδεση σε υπηρεσία Bus χρησιμοποιώντας AMQP 1.0.

## <a name="message-serialization"></a>Μήνυμα σειριοποίησης

Όταν χρησιμοποιείτε το προεπιλεγμένο πρωτόκολλο, η προεπιλεγμένη συμπεριφορά σειριοποίησης της βιβλιοθήκης του προγράμματος-πελάτη .NET είναι να χρησιμοποιήσετε τον τύπο [DataContractSerializer][] σειριοποίηση μια παρουσία [BrokeredMessage][] για μεταφορά ανάμεσα σε βιβλιοθήκη του προγράμματος-πελάτη και η υπηρεσία Bus υπηρεσίας. Όταν χρησιμοποιείτε τη λειτουργία μεταφοράς AMQP, τη βιβλιοθήκη προγράμματος-πελάτη χρησιμοποιεί το σύστημα τύπος AMQP σειριοποίησης του [μηνύματος όπου μεσολαβούν][BrokeredMessage] σε ένα μήνυμα AMQP. Αυτό σειριοποίησης ενεργοποιεί το μήνυμα για να λάβει και ερμηνεύεται από μια εφαρμογή λήψης που ενδεχομένως εκτελείται σε διαφορετική πλατφόρμα, για παράδειγμα, μια εφαρμογή Java που χρησιμοποιεί το API JMS για να αποκτήσετε πρόσβαση Bus υπηρεσίας.

Όταν δημιουργείτε μια παρουσία [BrokeredMessage][] , μπορείτε να παρέχετε ένα αντικείμενο .NET ως παράμετρο στην κατασκευή θα χρησιμοποιηθεί ως το σώμα του μηνύματος. Για τα αντικείμενα που μπορεί να αντιστοιχιστεί σε AMQP στοιχειώδεις τύπους, ο οργανισμός είναι σειριοποιημένο σε τύπους δεδομένων AMQP. Εάν το αντικείμενο δεν μπορεί να αντιστοιχιστεί απευθείας σε έναν τύπο στοιχειώδεις AMQP; Αυτό σημαίνει ότι ένας προσαρμοσμένος τύπος που ορίζονται από την εφαρμογή, στη συνέχεια, το αντικείμενο είναι σειριοποιημένο χρησιμοποιώντας το [DataContractSerializer][]και τα αλληλουχίας byte αποστέλλονται σε ένα μήνυμα AMQP δεδομένων.

Για να διευκολύνετε τη διαλειτουργικότητα με μη .NET προγράμματα-πελάτες, χρησιμοποιήστε μόνο τύπους .NET που μπορούν να σειριοποιηθούν απευθείας σε τύπους AMQP για το σώμα του μηνύματος. Ο παρακάτω πίνακας περιέχει λεπτομέρειες για αυτούς τους τύπους και την αντίστοιχη αντιστοίχιση στο σύστημα τύπου AMQP.

| Τύπος αντικειμένου σώμα .NET          | Τύπος αντιστοιχισμένα AMQP                          | Τύπος ενότητα σώμα AMQP                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| bool                           | δυαδική τιμή                                   | Τιμή AMQP                                                                                                                                                |
| byte                           | ubyte                                     | Τιμή AMQP                                                                                                                                                |
| USHORT                         | USHORT                                    | Τιμή AMQP                                                                                                                                                |
| UInt                           | UInt                                      | Τιμή AMQP                                                                                                                                                |
| ULONG                          | ULONG                                     | Τιμή AMQP                                                                                                                                                |
| SByte                          | byte                                      | Τιμή AMQP                                                                                                                                                |
| σύντομο                          | σύντομο                                     | Τιμή AMQP                                                                                                                                                |
| Int                            | Int                                       | Τιμή AMQP                                                                                                                                                |
| μεγάλη                           | μεγάλη                                      | Τιμή AMQP                                                                                                                                                |
| Αιώρηση                          | Αιώρηση                                     | Τιμή AMQP                                                                                                                                                |
| διπλά                         | διπλά                                    | Τιμή AMQP                                                                                                                                                |
| δεκαδικών ψηφίων                        | decimal128                                | Τιμή AMQP                                                                                                                                                |
| CHAR                           | CHAR                                      | Τιμή AMQP                                                                                                                                                |
| Ημερομηνίας και ώρας                       | χρονική σήμανση                                 | Τιμή AMQP                                                                                                                                                |
| GUID                           | UUID                                      | Τιμή AMQP                                                                                                                                                |
| byte]                         | δυαδικό                                    | Τιμή AMQP                                                                                                                                                |
| συμβολοσειρά                         | συμβολοσειρά                                    | Τιμή AMQP                                                                                                                                                |
| System.Collections.IList       | λίστα                                      | Τιμή AMQP: στοιχεία που περιέχονται στη συλλογή μπορεί να είναι μόνο αυτές που έχουν οριστεί σε αυτόν τον πίνακα.                                                             |
| System.array έχει ως                   | πίνακα                                     | Τιμή AMQP: στοιχεία που περιέχονται στη συλλογή μπορεί να είναι μόνο αυτές που έχουν οριστεί σε αυτόν τον πίνακα.                                                             |
| System.Collections.IDictionary | χάρτης                                       | Τιμή AMQP: στοιχεία που περιέχονται στη συλλογή μπορεί να είναι μόνο αυτές που έχουν οριστεί σε αυτόν τον πίνακα. Σημείωση: υποστηρίζονται μόνο κλειδιά συμβολοσειρά.                        |
| URI                            | Περιγράφεται συμβολοσειράς (ανατρέξτε στον παρακάτω πίνακα) | Τιμή AMQP                                                                                                                                                |
| DateTimeOffset                 | Περιγράφεται χρόνο (ανατρέξτε στον παρακάτω πίνακα)   | Τιμή AMQP                                                                                                                                                |
| Χρονικό διάστημα                       | Περιγράφεται χρόνο (ανατρέξτε στο ακόλουθο)         | Τιμή AMQP                                                                                                                                                |
| Ροή                         | δυαδικό                                    | Τα δεδομένα AMQP (μπορεί να είναι πολλά). Οι ενότητες δεδομένων περιέχουν τα ανεπεξέργαστα byte ανάγνωσης από το αντικείμενο ροής.                                                           |
| Άλλο αντικείμενο                   | δυαδικό                                    | Τα δεδομένα AMQP (μπορεί να είναι πολλά). Περιέχει το αλληλουχίας δυαδικό αρχείο του αντικειμένου που χρησιμοποιεί το DataContractSerializer ή ένα πρόγραμμα σειριοποίησης που παρέχονται από την εφαρμογή. |

| Τύπος .NET      | Αντιστοιχισμένα AMQP περιγράφεται τύπου                                                                                              | Σημειώσεις                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Χρονικό διάστημα       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Μη υποστηριζόμενες δυνατότητες, περιορισμούς και συμπεριφοράς διαφορές

Οι παρακάτω δυνατότητες του API υπηρεσίας Bus .NET δεν υποστηρίζονται αυτήν τη στιγμή κατά τη χρήση AMQP:

-   Συναλλαγές

-   Αποστολή μέσω μεταφοράς προορισμού

Υπάρχουν επίσης ορισμένες μικρές διαφορές στην συμπεριφορά του API υπηρεσίας Bus .NET κατά τη χρήση AMQP, σε σύγκριση με το προεπιλεγμένο πρωτόκολλο:

-   Η ιδιότητα [OperationTimeout][] παραβλέπεται.

-   `MessageReceiver.Receive(TimeSpan.Zero)`εφαρμόζεται ως `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Ολοκλήρωση μηνύματα με διακριτικά κλείδωμα μπορεί να γίνει μόνο από το δέκτες μηνύματος που λάβατε αρχικά τα μηνύματα.

## <a name="controlling-amqp-protocol-settings"></a>Έλεγχος ρυθμίσεων πρωτοκόλλου AMQP

Τα API .NET εκθέτουν διάφορες ρυθμίσεις για τον έλεγχο της συμπεριφοράς του πρωτοκόλλου AMQP:

-   **MessageReceiver.PrefetchCount**: ελέγχει το αρχικό πιστωτικό εφαρμόζεται σε μια σύνδεση. Η προεπιλογή είναι 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: το μέγιστο μέγεθος του πλαισίου AMQP που διέθετε κατά τη διάρκεια της διαπραγμάτευσης κατά τη σύνδεση στοιχείων ελέγχου Άνοιγμα ώρα. Η προεπιλογή είναι 65.536 byte.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Εάν batchable μεταφορές, αυτή η τιμή καθορίζει το μέγιστο καθυστέρηση για την αποστολή διατάξεων. Μεταβιβαστεί από αποστολείς/δέκτες από προεπιλογή. Μεμονωμένες αποστολέα/ακουστικό να παρακάμψετε την προεπιλογή, που είναι 20 χιλιοστά του δευτερολέπτου.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: ελέγχει αν έχουν δημιουργηθεί AMQP συνδέσεις μέσω σύνδεσης SSL. Η προεπιλογή είναι **true**.

## <a name="next-steps"></a>Επόμενα βήματα

Είστε έτοιμοι για να μάθετε περισσότερα; Επισκεφθείτε τις παρακάτω συνδέσεις:

- [Επισκόπηση AMQP Bus υπηρεσίας]
- [Υποστήριξη AMQP 1.0 Bus υπηρεσίας διαμερίσματα ουρές και θέματα]
- [AMQP στην υπηρεσία Bus για τον Windows Server]

  [Γρήγορα αποτελέσματα με το ουρές Bus υπηρεσίας]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Πύλη του Azure]: https://portal.azure.com
[Επισκόπηση AMQP Bus υπηρεσίας]: service-bus-amqp-overview.md
[Υποστήριξη AMQP 1.0 Bus υπηρεσίας διαμερίσματα ουρές και θέματα]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP στην υπηρεσία Bus για τον Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
