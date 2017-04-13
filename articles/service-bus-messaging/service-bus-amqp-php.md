<properties 
    pageTitle="Λειτουργία Bus και PHP με AMQP 1.0 | Microsoft Azure"
    description="Χρήση υπηρεσίας Bus από PHP με AMQP."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-php-with-amqp-10"></a>Χρήση υπηρεσίας Bus από PHP με AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton PHP είναι μια σύνδεση γλώσσας PHP να Proton-C; Αυτό σημαίνει ότι έχει υλοποιηθεί Proton PHP ως περίβλημα γύρω από μια μηχανή υλοποιηθεί σε C.

## <a name="downloading-the-proton-client-library"></a>Τη λήψη της βιβλιοθήκης Proton προγράμματος-πελάτη

Μπορείτε να λάβετε Proton-C και τις σχετικές συνδέσεις (συμπεριλαμβανομένων των PHP) από [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Η λήψη είναι σε μορφή κώδικα προέλευσης. Για να δημιουργήσετε τον κώδικα, ακολουθήστε τις οδηγίες που περιέχονται στο πακέτο λήψης.

> [AZURE.IMPORTANT] Προς το παρόν, η υποστήριξη SSL στο Proton-C είναι διαθέσιμη μόνο για τα λειτουργικά συστήματα Linux. Επειδή Azure Service Bus απαιτεί τη χρήση του SSL, Proton-C (και τις συνδέσεις γλώσσα) μπορεί να χρησιμοποιηθεί μόνο για πρόσβαση στην υπηρεσία Bus από Linux αυτήν τη στιγμή. Εργασία για να ενεργοποιήσετε την Proton-C με SSL σε Windows βρίσκεται σε εξέλιξη, επομένως ελέγχου ξανά συχνά για ενημερώσεις.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Εργασία με ουρές, θέματα και συνδρομές από PHP Bus υπηρεσίας

Ο ακόλουθος κώδικας δείχνει τον τρόπο αποστολή και λήψη μηνυμάτων από μια υπηρεσία Bus μηνυμάτων οντότητα.

### <a name="sending-messages-using-proton-php"></a>Αποστολή μηνυμάτων μέσω Proton PHP

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε ένα μήνυμα σε μια υπηρεσία Bus μηνυμάτων οντότητα.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Λαμβάνετε μηνύματα χρησιμοποιώντας το Proton PHP

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να λάβετε ένα μήνυμα από μια υπηρεσία Bus μηνυμάτων οντότητα.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Ανταλλαγή μηνυμάτων μεταξύ .NET και Proton PHP

### <a name="application-properties"></a>Ιδιότητες εφαρμογής

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP για την εξυπηρέτηση Bus .NET APIs

Μηνύματα proton PHP υποστηρίζουν ιδιότητες εφαρμογής από τους παρακάτω τύπους: **Ακέραιος**, **διπλά**, **Boolean**, **συμβολοσειρά**και **αντικείμενο**. Ο ακόλουθος κώδικας PHP δείχνει πώς μπορείτε να ορίσετε ιδιότητες για ένα μήνυμα με τη χρήση κάθε έναν από αυτούς τους τύπους ιδιοτήτων.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

Στο τα API .NET Bus υπηρεσίας, ιδιότητες εφαρμογής μήνυμα μεταφέρονται στη συλλογή **Properties** του [BrokeredMessage][]. Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να διαβάσετε τις ιδιότητες εφαρμογής ενός μηνύματος που λάβατε από ένα πρόγραμμα-πελάτη PHP.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

Ο παρακάτω πίνακας αντιστοιχίζει τους τύπους ιδιοτήτων PHP με τους τύπους ιδιοτήτων .NET.

| Τύπος ιδιότητας PHP | Τύπος ιδιότητας .NET |
|-------------------|--------------------|
| Ακέραιος αριθμός           | Int                |
| διπλά            | διπλά             |
| δυαδική τιμή           | bool               |
| συμβολοσειρά            | συμβολοσειρά             |
| αντικείμενο            | Αντικείμενο             |

#### <a name="service-bus-net-apis-to-php"></a>Υπηρεσία Bus .NET APIs να PHP

Ο τύπος [BrokeredMessage][] υποστηρίζει ιδιότητες εφαρμογής από τους παρακάτω τύπους: **byte**, **sbyte**, **char**, **Σύντομη**, **ushort**, **int**, **uint**, **μεγάλο**, **ulong**, **Αιώρηση**, **διπλά**, **δεκαδικών ψηφίων**, **bool**, **Guid**, **συμβολοσειρά**, **Uri**, **ημερομηνίας/ώρας**, **DateTimeOffset**, και **χρονικό διάστημα**. Ο ακόλουθος κώδικας .NET δείχνει πώς μπορείτε να ορίσετε ιδιότητες για ένα αντικείμενο [BrokeredMessage][] χρησιμοποιώντας κάθε έναν από αυτούς τους τύπους ιδιοτήτων.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Ο ακόλουθος κώδικας PHP δείχνει πώς μπορείτε να διαβάσετε τις ιδιότητες εφαρμογής ενός μηνύματος που λάβατε από ένα πρόγραμμα-πελάτη .NET Bus υπηρεσίας.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

Ο παρακάτω πίνακας αντιστοιχίζει τους τύπους ιδιοτήτων .NET με τους τύπους ιδιοτήτων PHP.

| Τύπος ιδιότητας .NET | Τύπος ιδιότητας PHP | Σημειώσεις                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| SByte              | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| CHAR               | Κατά ένα χαρακτήρα              | Proton PHP τάξης                                                                                                                                                    |
| σύντομο              | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| USHORT             | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| Int                | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| UInt               | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| μεγάλη               | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| ULONG              | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| Αιώρηση              | διπλά            | -                                                                                                                                                                     |
| διπλά             | διπλά            | -                                                                                                                                                                     |
| δεκαδικών ψηφίων            | συμβολοσειρά            | Δεκαδικό δεν υποστηρίζεται αυτήν τη στιγμή με Proton.                                                                                                                     |
| bool               | δυαδική τιμή           | -                                                                                                                                                                     |
| GUID               | UUID              | Proton PHP τάξης                                                                                                                                                    |
| συμβολοσειρά             | συμβολοσειρά            | -                                                                                                                                                                     |
| Ημερομηνίας και ώρας           | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | Αντιστοίχιση τύπο AMQP DateTimeOffset.UtcTicks:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Χρονικό διάστημα           | DescribedType     | Αντιστοίχιση τύπο AMQP Timespan.Ticks:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Αντιστοίχιση τύπο AMQP Uri.AbsoluteUri:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Τυπικές ιδιότητες

Οι παρακάτω πίνακες δείχνουν την αντιστοίχιση μεταξύ των ιδιοτήτων τυπικό μήνυμα Proton PHP και τις ιδιότητες τυπικό μήνυμα [BrokeredMessage][] .

| Proton PHP           | Υπηρεσία Bus .NET         | Σημειώσεις                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Διαρκή              | δ/υ                      | Υπηρεσία Bus υποστηρίζει μόνο διαρκή μηνύματα.          |
| Προτεραιότητα             | δ/υ                      | Υπηρεσία Bus υποστηρίζει μόνο ένα μεμονωμένο μήνυμα προτεραιότητα. |
| TTL                  | Message.TimeToLive       | Μετατροπή, TTL Proton PHP έχει οριστεί σε χιλιοστά του δευτερολέπτου.   |
| πρώτη\_εξαγοράζουσα εταιρεία      | -                          | -                                                          |
| παράδοση\_count      | -                          | -                                                          |
| Αναγνωριστικό                   | Message.Id               | -                                                          |
| χρήστης\_αναγνωριστικό             | -                          | -                                                          |
| Διεύθυνση              | Message.To               | -                                                          |
| Θέμα              | Message.Label            | -                                                          |
| Απάντηση\_να            | Message.ReplyTo          | -                                                          |
| συσχέτισης\_αναγνωριστικό      | Message.CorrelationId    | -                                                          |
| περιεχόμενο\_τύπου        | Message.ContentType      | -                                                          |
| περιεχόμενο\_κωδικοποίηση    | δ/υ                      | -                                                          |
| Λήξη\_ώρα         | Message.ExpiresAtUTC     | -                                                          |
| Δημιουργία\_ώρα       | δ/υ                      | -                                                          |
| ομάδα\_αναγνωριστικό            | Message.SessionId        | -                                                          |
| ομάδα\_ακολουθίας      | -                          | -                                                          |
| Απάντηση\_να\_ομάδα\_αναγνωριστικό | Message.ReplyToSessionId | -                                                          |
| Μορφή               | δ/υ                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Υπηρεσία Bus .NET APIs να Proton PHP

| Υπηρεσία Bus .NET        | Proton PHP                                             | Σημειώσεις                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Μήνυμα -\>περιεχόμενο\_τύπου                                | -                                                        |
| CorrelationId           | Μήνυμα -\>συσχέτισης\_αναγνωριστικό                              | -                                                        |
| EnqueuedTimeUtc         | Μήνυμα -\>σχόλια [x-επιλέξετε-που τοποθετούνται στην ουρά-ώρα]             | -                                                        |
| Ετικέτα                   | Μήνυμα -\>θέμα                                      | -                                                        |
| Αναγνωριστικού μηνύματος               | Μήνυμα -\>αναγνωριστικό                                           | -                                                        |
| Τη                 | Μήνυμα -\>απάντηση\_να                                    | -                                                        |
| ReplyToSessionId        | Μήνυμα -\>απάντηση\_να\_ομάδα\_αναγνωριστικό                         | -                                                        |
| ScheduledEnqueueTimeUtc | Μήνυμα -\>σχόλια ["x-επιλέξετε-προγραμματισμένη-enqueue-ώρας"] | -                                                        |
| Αναγνωριστικό περιόδου λειτουργίας               | Μήνυμα -\>ομάδα\_αναγνωριστικό                                    | -                                                        |
| Το TimeToLive              | Μήνυμα -\>ttl                                          | Μετατροπή, TTL Proton PHP έχει οριστεί σε χιλιοστά του δευτερολέπτου. |
| Για να                      | Μήνυμα -\>διεύθυνση                                      | -                                                        |

## <a name="next-steps"></a>Επόμενα βήματα

Είστε έτοιμοι για να μάθετε περισσότερα; Επισκεφθείτε τις παρακάτω συνδέσεις:

- [Επισκόπηση AMQP Bus υπηρεσίας]
- [AMQP στην υπηρεσία Bus για τον Windows Server]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP στην υπηρεσία Bus για τον Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[Επισκόπηση AMQP Bus υπηρεσίας]: service-bus-amqp-overview.md
