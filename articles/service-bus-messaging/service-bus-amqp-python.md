<properties 
    pageTitle="Λειτουργία Bus και Python με AMQP 1.0 | Microsoft Azure"
    description="Χρήση υπηρεσίας Bus από Python με AMQP."
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

# <a name="using-service-bus-from-python-with-amqp-10"></a>Χρήση υπηρεσίας Bus από Python με AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton Python είναι μια σύνδεση γλώσσας Python να Proton-C; Αυτό σημαίνει ότι έχει υλοποιηθεί Proton Python ως περίβλημα γύρω από μια μηχανή υλοποιηθεί σε C.

## <a name="download-the-proton-client-library"></a>Κάντε λήψη της βιβλιοθήκης Proton προγράμματος-πελάτη

Μπορείτε να λάβετε Proton-C και τις σχετικές συνδέσεις (συμπεριλαμβανομένων των Python) από [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Η λήψη είναι σε μορφή κώδικα προέλευσης. Για να δημιουργήσετε τον κώδικα, ακολουθήστε τις οδηγίες που περιέχονται σε το πακέτο που έχετε λάβει.

Σημειώστε ότι προς το παρόν, η υποστήριξη SSL στο Proton-C είναι διαθέσιμη μόνο για τα λειτουργικά συστήματα Linux. Επειδή Azure Service Bus απαιτεί τη χρήση του SSL, Proton-C (και τις συνδέσεις γλώσσα) μπορεί να χρησιμοποιηθεί μόνο για πρόσβαση στην υπηρεσία Bus από Linux αυτήν τη στιγμή. Εργασία για να ενεργοποιήσετε την Proton-C με SSL στα Windows είναι σε εξέλιξη, συνεπώς ελέγχου ξανά συχνά για ενημερώσεις.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Εργασία με ουρές, θέματα και συνδρομές από Python Bus υπηρεσίας

Ο ακόλουθος κώδικας δείχνει τον τρόπο αποστολή και λήψη μηνυμάτων από μια υπηρεσία Bus μηνυμάτων οντότητα.

### <a name="send-messages-using-proton-python"></a>Αποστολή μηνυμάτων με τη χρήση Proton Python

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε ένα μήνυμα σε μια υπηρεσία Bus μηνυμάτων οντότητα.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Λαμβάνετε μηνύματα χρησιμοποιώντας το Proton Python

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να λάβετε ένα μήνυμα από μια υπηρεσία Bus μηνυμάτων οντότητα.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Ανταλλαγή μηνυμάτων μεταξύ .NET και Proton Python

### <a name="application-properties"></a>Ιδιότητες εφαρμογής

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python για να εξυπηρετήσει Bus .NET APIs

Μηνύματα proton Python υποστηρίζουν ιδιότητες εφαρμογής από τους παρακάτω τύπους: **int**, **μεγάλο**, **Αιώρηση**, **uuid**, **bool**, **συμβολοσειρά**. Ο ακόλουθος κώδικας Python δείχνει πώς μπορείτε να ορίσετε ιδιότητες για ένα μήνυμα με τη χρήση κάθε έναν από αυτούς τους τύπους ιδιοτήτων.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

Στην υπηρεσία Bus .NET API, ιδιότητες εφαρμογής μήνυμα μεταφέρονται στη συλλογή **Properties** του [BrokeredMessage][]. Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να διαβάσετε τις ιδιότητες εφαρμογής ενός μηνύματος που λάβατε από ένα πρόγραμμα-πελάτη Python.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Ο παρακάτω πίνακας αντιστοιχίζει τους τύπους ιδιοτήτων Python με τους τύπους ιδιοτήτων .NET.

| Τύπος ιδιότητας Python | Τύπος ιδιότητας .NET |
|----------------------|--------------------|
| Int                  | Int                |
| Αιώρηση                | διπλά             |
| μεγάλη                 | Int64              |
| UUID                 | GUID               |
| bool                 | bool               |
| συμβολοσειρά               | συμβολοσειρά             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Υπηρεσία Bus .NET APIs να Proton Python

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
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Ο ακόλουθος κώδικας Python δείχνει πώς μπορείτε να διαβάσετε τις ιδιότητες εφαρμογής ενός μηνύματος που λάβατε από ένα πρόγραμμα-πελάτη .NET Bus υπηρεσίας.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

Ο παρακάτω πίνακας αντιστοιχίζει τους τύπους ιδιοτήτων .NET με τους τύπους ιδιοτήτων Python.

| Τύπος ιδιότητας .NET | Τύπος ιδιότητας Python | Σημειώσεις                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | Int                  | -                                                                                                                                                                     |
| SByte              | Int                  | -                                                                                                                                                                     |
| CHAR               | CHAR                 | Proton Python τάξης                                                                                                                                                 |
| σύντομο              | Int                  | -                                                                                                                                                                     |
| USHORT             | Int                  | -                                                                                                                                                                     |
| Int                | Int                  | -                                                                                                                                                                     |
| UInt               | Int                  | -                                                                                                                                                                     |
| μεγάλη               | Int                  | -                                                                                                                                                                     |
| ULONG              | μεγάλη                 | Proton Python τάξης                                                                                                                                                 |
| Αιώρηση              | Αιώρηση                | -                                                                                                                                                                     |
| διπλά             | Αιώρηση                | -                                                                                                                                                                     |
| δεκαδικών ψηφίων            | Συμβολοσειρά               | Δεκαδικό δεν υποστηρίζεται αυτήν τη στιγμή με Proton.                                                                                                                     |
| bool               | bool                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Proton Python τάξης                                                                                                                                                 |
| συμβολοσειρά             | συμβολοσειρά               | -                                                                                                                                                                     |
| Ημερομηνίας και ώρας           | χρονική σήμανση            | Proton Python τάξης                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | Αντιστοίχιση τύπο AMQP DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Χρονικό διάστημα           | DescribedType        | Αντιστοίχιση τύπο AMQP Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Αντιστοίχιση τύπο AMQP Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Τυπικές ιδιότητες

Οι παρακάτω πίνακες δείχνουν την αντιστοίχιση μεταξύ των ιδιοτήτων τυπικό μήνυμα Proton Python και τις ιδιότητες τυπικό μήνυμα [BrokeredMessage][] .

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python για να εξυπηρετήσει Bus .NET APIs

| Proton Python        | Υπηρεσία Bus .NET         | Σημειώσεις                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| διαρκή              | δ/υ                      | Υπηρεσία Bus υποστηρίζει μόνο διαρκή μηνύματα.               |
| προτεραιότητα             | δ/υ                      | Υπηρεσία Bus υποστηρίζει μόνο ένα μεμονωμένο μήνυμα προτεραιότητα.      |
| TTL                  | Message.TimeToLive       | Μετατροπή, TTL Proton Python έχει οριστεί σε χιλιοστά του δευτερολέπτου. |
| πρώτη\_εξαγοράζουσα εταιρεία      | δ/υ                      | -                                                           |
| παράδοση\_count      | δ/υ                      | -                                                           |
| Αναγνωριστικό                   | Message.MessageID        | -                                                           |
| χρήστης\_αναγνωριστικό             | δ/υ                      | -                                                           |
| διεύθυνση              | Message.To               | -                                                           |
| θέμα              | Message.Label            | -                                                           |
| Απάντηση\_να            | Message.ReplyTo          | -                                                           |
| συσχέτισης\_αναγνωριστικό      | Message.CorrelationID    | -                                                           |
| περιεχόμενο\_τύπου        | Message.ContentType      | -                                                           |
| περιεχόμενο\_κωδικοποίηση    | δ/υ                      | -                                                           |
| Λήξη\_ώρα         | δ/υ                      | -                                                           |
| Δημιουργία\_ώρα       | δ/υ                      | -                                                           |
| ομάδα\_αναγνωριστικό            | Message.SessionId        | -                                                           |
| ομάδα\_ακολουθίας      | δ/υ                      | -                                                           |
| Απάντηση\_να\_ομάδα\_αναγνωριστικό | Message.ReplyToSessionId | -                                                           |
| μορφή               | δ/υ                      | -                                                           |

| Υπηρεσία Bus .NET        | Proton                       | Σημειώσεις                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.Content\_τύπου        | -                                                           |
| CorrelationId           | Message.Correlation\_αναγνωριστικό      | -                                                           |
| EnqueuedTimeUtc         | δ/υ                          | -                                                           |
| Ετικέτα                   | Message.Subject              | -                                                           |
| Αναγνωριστικού μηνύματος               | Message.ID                   | -                                                           |
| Τη                 | Message.Reply\_να            | -                                                           |
| ReplyToSessionId        | Message.Reply\_να\_ομάδα\_αναγνωριστικό | -                                                           |
| ScheduledEnqueueTimeUtc | δ/υ                          | -                                                           |
| Αναγνωριστικό περιόδου λειτουργίας               | Message.Group\_αναγνωριστικό            | -                                                           |
| Το TimeToLive              | Message.TTL                  | Μετατροπή, TTL Proton Python έχει οριστεί σε χιλιοστά του δευτερολέπτου. |
| Για να                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Επόμενα βήματα

Είστε έτοιμοι για να μάθετε περισσότερα; Επισκεφθείτε τις παρακάτω συνδέσεις:

- [Επισκόπηση AMQP Bus υπηρεσίας]
- [AMQP στην υπηρεσία Bus για τον Windows Server]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP στην υπηρεσία Bus για τον Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[Επισκόπηση AMQP Bus υπηρεσίας]: service-bus-amqp-overview.md
