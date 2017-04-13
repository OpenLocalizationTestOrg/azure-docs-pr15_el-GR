<properties 
    pageTitle="Λειτουργία Bus και Java με AMQP 1.0 | Microsoft Azure"
    description="Χρήση υπηρεσίας Bus από Java με AMQP"
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

# <a name="use-service-bus-from-java-with-amqp-10"></a>Χρήση υπηρεσίας Bus από Java με AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Η υπηρεσία μήνυμα Java (JMS) είναι ένα τυπικό API για την εργασία με ενδιάμεσο την πλατφόρμα Java. Microsoft Azure Bus υπηρεσίας έχει ελεγχθεί με το 1.0 AMQP βάσει JMS βιβλιοθήκη προγράμματος-πελάτη που αναπτύχθηκε από το έργο Apache Qpid. Αυτή η βιβλιοθήκη υποστηρίζει την πλήρη API 1.1 JMS και μπορούν να χρησιμοποιηθούν με οποιαδήποτε συμβατή υπηρεσία ανταλλαγής μηνυμάτων AMQP 1.0. Αυτό το σενάριο υποστηρίζεται επίσης στην [Υπηρεσία Bus για Windows Server](https://msdn.microsoft.com/library/dn282144.aspx) (εσωτερικής εγκατάστασης υπηρεσίας Bus). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [AMQP στην υπηρεσία Bus για Windows Server][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Κάντε λήψη της βιβλιοθήκης Apache Qpid AMQP 1.0 JMS προγράμματος-πελάτη

Για πληροφορίες σχετικά με τη λήψη την πιο πρόσφατη έκδοση της βιβλιοθήκης του προγράμματος-πελάτη Apache Qpid JMS AMQP 1.0, επισκεφθείτε [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

Πρέπει να προσθέσετε τα ακόλουθα τέσσερα ΒΆΖΟ αρχεία από το αρχείο αρχειοθέτησης διανομής Apache Qpid JMS AMQP 1.0 το CLASSPATH Java όταν η δημιουργία και εκτέλεση εφαρμογών JMS με Bus υπηρεσίας:

-   geronimo jms\_1.1\_.jar προδιαγραφή-[έκδοση]

-   qpid-amqp-1-0-Client-[Version].jar

-   qpid-amqp-1-0-Client-jms-[Version].jar

-   qpid-amqp-1-0-Common-[Version].jar

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Εργασία με ουρές, θέματα και συνδρομές από JMS Bus υπηρεσίας

### <a name="java-naming-and-directory-interface-jndi"></a>Ονομασία Java και καταλόγου περιβάλλοντος εργασίας (JNDI)

JMS χρησιμοποιεί την ονομασία Java και καταλόγου περιβάλλοντος εργασίας (JNDI) για να δημιουργήσετε ένα διαχωρισμό μεταξύ λογικά ονόματα και ονόματα των φυσικών. Δύο τύποι αντικειμένων JMS επιλύονται χρησιμοποιώντας JNDI: **ConnectionFactory** και **προορισμού**. JNDI χρησιμοποιεί ένα μοντέλο υπηρεσίας παροχής στο οποίο μπορείτε να συνδέσετε διάφορες υπηρεσίες καταλόγων για το χειρισμό των καθηκόντων ανάλυση ονόματος. Η βιβλιοθήκη Apache Qpid JMS AMQP 1.0 συνοδεύεται από ένα απλό ιδιότητες παροχής JNDI βασίζεται στο αρχείο που έχει ρυθμιστεί με ένα αρχείο κειμένου.

Η υπηρεσία παροχής JNDI Qpid ιδιότητες αρχείου έχει ρυθμιστεί χρησιμοποιώντας ένα αρχείο ιδιότητες από την εξής μορφή:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>Ρύθμιση παραμέτρων του εργοστασίου σύνδεσης

Η εγγραφή χρησιμοποιείται για τον ορισμό ενός **ConnectionFactory** στην υπηρεσία παροχής JNDI αρχείο Qpid ιδιότητες είναι την εξής μορφή:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Όπου `[jndi\_name]` και `[ConnectionURL]` έχουν την εξής σημασία:

| Όνομα            | Σημασία                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | Το λογικό όνομα του εργοστασίου σύνδεσης. Αυτό το όνομα επιλύεται στην εφαρμογή Java χρησιμοποιώντας το JNDI `IntialContext.lookup()` μέθοδο. |   |   |   |   |
| `[ConnectionURL]` | Μια διεύθυνση URL που παρέχει τη βιβλιοθήκη JMS με τις πληροφορίες που απαιτούνται για το broker AMQP.                                                      |   |   |   |   |

Η μορφή για τη σύνδεση διεύθυνση URL είναι ως εξής:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Όπου `[namespace]`, `[username]`, και `[password]` έχουν την εξής σημασία:

| Όνομα          | Σημασία                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | Ο χώρος ονομάτων Bus υπηρεσίας που λαμβάνονται από την [πύλη του Azure][].                      |   |   |   |   |
| `[username]`  | Το όνομα συσχετισμών Ασφαλείας Bus υπηρεσίας κλειδιού που λαμβάνονται από την [πύλη του Azure][].                    |   |   |   |   |
| `[password]`  | Κωδικοποίηση URL φόρμας του κλειδιού συσχετισμών Ασφαλείας Bus υπηρεσίας που λαμβάνονται από την [πύλη του Azure][]. |   |   |   |   |

> [AZURE.NOTE] Πρέπει να κωδικοποιήσετε διεύθυνση URL του κωδικού πρόσβασης με μη αυτόματο τρόπο. Ένα βοηθητικό πρόγραμμα κωδικοποίησης χρήσιμες διεύθυνση URL είναι διαθέσιμη από την [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

Για παράδειγμα, εάν οι πληροφορίες που λαμβάνονται από την πύλη είναι ως εξής:

| Namespace:   | Test.servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Όνομα εκδότη: | RootManageSharedAccessKey                                        |
| Εκδότης κλειδί:  | αβγδεζη |

Στη συνέχεια, για να ορίσετε ένα αντικείμενο **ConnectionFactory** με το όνομα `SBCONNECTIONFACTORY`, η συμβολοσειρά ρύθμισης παραμέτρων πρέπει να είναι ως εξής:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Ρύθμιση παραμέτρων των προορισμών

Η εγγραφή που καθορίζει έναν προορισμό στην υπηρεσία παροχής JNDI αρχείο Qpid ιδιότητες είναι την εξής μορφή:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Όπου `[jndi\_name]` και `[physical\_name]` έχουν την εξής σημασία:

| Όνομα              | Σημασία                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | Το λογικό όνομα του προορισμού. Αυτό είναι το όνομα επιλύεται στην εφαρμογή Java χρησιμοποιώντας το JNDI `IntialContext.lookup()` μέθοδο. |
| `[physical\name]` | Το όνομα της οντότητας Bus υπηρεσία στην οποία η εφαρμογή αποστέλλει ή λαμβάνει μηνύματα.                                                  |

Λάβετε υπόψη τα εξής:

- Το `[physical\name]` τιμή μπορεί να είναι μια υπηρεσία Bus ουρά ή ένα θέμα.
- Κατά τη λήψη από μια συνδρομή θέμα Bus υπηρεσία, το φυσικό όνομα που καθορίζεται στο JNDI πρέπει να είναι το όνομα του θέματος. Όταν δημιουργείται η συνδρομή διαρκή στον κώδικα εφαρμογής JMS παρέχονται στο όνομα της συνδρομής.
- Επίσης, είναι δυνατό να χειριστείτε μια συνδρομή θέμα υπηρεσία Bus ως ουρά JMS. Υπάρχουν πολλά πλεονεκτήματα αυτής της προσέγγισης: τον ίδιο κωδικό ακουστικό μπορούν να χρησιμοποιηθούν για το θέμα συνδρομές και ουρές και όλα τα στοιχεία διεύθυνσης (τα ονόματα θέμα και συνδρομής) είναι externalized στο αρχείο ιδιότητες.
- Για να αντιμετωπιστεί μια συνδρομή θέμα υπηρεσία Bus ως ουρά JMS, την καταχώρηση στο αρχείο ιδιότητες πρέπει να είναι μέρος της φόρμας: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Για να ορίσετε μια λογική προορισμού JMS με το όνομα "ΘΈΜΑ" που αντιστοιχεί σε ένα θέμα Bus υπηρεσίας με το όνομα "θέμα1;", η εγγραφή στο αρχείο ιδιότητες θα είναι ως εξής:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Αποστολή μηνυμάτων με τη χρήση JMS

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε ένα μήνυμα σε ένα θέμα Bus υπηρεσίας. Θεωρείται που `SBCONNECTIONFACTORY` και `TOPIC` ορίζονται σε ένα αρχείο ρύθμισης παραμέτρων **servicebus.properties** , όπως περιγράφεται στην προηγούμενη ενότητα.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Λαμβάνετε μηνύματα χρησιμοποιώντας το JMS

Τον ακόλουθο κώδικα εμφανίζει `how` για να λάβετε ένα μήνυμα από μια συνδρομή θέμα Bus υπηρεσίας. Θεωρείται που `SBCONNECTIONFACTORY` και το ΘΈΜΑ ορίζονται σε ένα αρχείο ρύθμισης παραμέτρων **servicebus.properties** , όπως περιγράφεται στην προηγούμενη ενότητα. Θεωρείται επίσης ότι είναι το όνομα της συνδρομής `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Οδηγίες για τη δημιουργία ισχυρές εφαρμογές

Η προδιαγραφή JMS καθορίζει πώς πρέπει να υπογράψει κάποιος το συμβόλαιο εξαίρεση των μεθόδων API και κώδικα της εφαρμογής για το χειρισμό οι εξαιρέσεις. Εδώ θα βρείτε ορισμένα άλλα σημεία πρέπει να λάβετε υπόψη σχετικά με τις χειρισμού εξαιρέσεων:

-   Καταχωρήστε μια **ExceptionListener** τη σύνδεση JMS χρησιμοποιώντας **connection.setExceptionListener**. Αυτή η δυνατότητα επιτρέπει ένας υπολογιστής-πελάτης να ειδοποιείστε πρόβλημα ασύγχρονα. Αυτή η ειδοποίηση είναι ιδιαίτερα σημαντικό για τις συνδέσεις που μόνο εκμετάλλευση μηνυμάτων, όπως έχουν κάποιος άλλος τρόπος για να μάθετε ότι απέτυχε η σύνδεσή τους. Το **ExceptionListener** ονομάζεται εάν υπάρχει πρόβλημα με το υποκείμενο AMQP σύνδεσης, την περίοδο λειτουργίας ή να συνδέσετε. Σε αυτήν την περίπτωση, το πρόγραμμα εφαρμογής πρέπει να δημιουργήσετε ξανά το **JMS σύνδεσης**, **την περίοδο λειτουργίας**, **MessageProducer** και **MessageConsumer** αντικειμένων από την αρχή.

-   Για να επαληθεύσετε ότι ένα μήνυμα έχει αποσταλεί με επιτυχία από μια **MessageProducer** σε μια υπηρεσία Bus οντότητα, βεβαιωθείτε ότι η εφαρμογή έχει ρυθμιστεί με το **qpid.sync\_δημοσίευση** σύνολο ιδιοτήτων συστήματος. Μπορείτε να το κάνετε με την εκκίνηση του προγράμματος με το **-Dqpid.sync\_δημοσίευση = true** Εικονική Java σύνολο επιλογών στην γραμμή εντολών κατά την εκκίνηση της εφαρμογής. Ρύθμιση αυτής της επιλογής ρυθμίζει τις παραμέτρους της βιβλιοθήκης για να επιστρέψετε δεν από την κλήση αποστολή μέχρι να έχει ληφθεί επιβεβαίωσης που το μήνυμα έχει γίνει αποδεκτή από Bus υπηρεσίας. Εάν παρουσιαστεί κάποιο πρόβλημα κατά τη διάρκεια της λειτουργίας αποστολή, ενεργοποιείται μια **JMSException** . Υπάρχουν δύο πιθανές αιτίες: 
    1. Εάν το πρόβλημα είναι λόγω Bus υπηρεσίας απορρίπτει το συγκεκριμένο μήνυμα που στέλνεται, θα ενεργοποιείται μια εξαίρεση **MessageRejectedException** . Αυτό το σφάλμα είναι προσωρινό, ή λόγω κάποιο πρόβλημα με το μήνυμα. Τα προτεινόμενα ενέργεια είναι να κάνετε διάφορες προσπαθεί να επαναλάβει τη λειτουργία με ορισμένες λογικής πίσω απενεργοποίηση. Εάν το πρόβλημα δεν επιλυθεί, θα πρέπει να εγκαταλειφθούν το μήνυμα με το μήνυμα σφάλματος συνδεδεμένοι τοπικά. Δεν χρειάζεται να δημιουργήσετε ξανά τα αντικείμενα **JMS σύνδεσης**, **την περίοδο λειτουργίας**ή **MessageProducer** σε αυτήν την περίπτωση. 
    2. Εάν το πρόβλημα είναι λόγω Bus υπηρεσίας κλείσετε τη σύνδεση AMQP, θα ενεργοποιείται μια εξαίρεση **InvalidDestinationException** . Αυτό μπορεί να οφείλεται σε ένα πρόβλημα μεταβατικές ή λόγω οντότητας του μηνύματος που διαγράφεται. Σε κάθε περίπτωση, πρέπει να δημιουργηθούν ξανά τα αντικείμενα **JMS σύνδεσης**, **την περίοδο λειτουργίας**και **MessageProducer** . Εάν η συνθήκη σφάλματος ήταν μεταβατικές, στη συνέχεια, αυτή η λειτουργία τελικά θα ολοκληρωθεί με επιτυχία. Εάν η οντότητα έχει διαγραφεί, την αποτυχία θα μόνιμο.

## <a name="messaging-between-net-and-jms"></a>Ανταλλαγή μηνυμάτων μεταξύ .NET και JMS

### <a name="message-bodies"></a>Σώμα των μηνυμάτων

JMS ορίζει πέντε τύποι διαφορετικό μήνυμα: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**και **TextMessage**. Το API της υπηρεσίας Bus .NET έχει έναν τύπο μήνυμα, [BrokeredMessage][].

#### <a name="jms-to-service-bus-net-api"></a>JMS σε υπηρεσία Bus .NET API

Οι ενότητες που ακολουθούν δείχνουν τον τρόπο για την εκμετάλλευση μηνύματα από κάθε έναν από τους τύπους JMS μηνυμάτων από το .NET. Παράδειγμα **ObjectMessage** δεν έχει περιλαμβάνονται, όπως το σώμα του ένα **ObjectMessage** περιέχει ένα αντικείμενο σειριοποιηθεί στο τη γλώσσα προγραμματισμού Java, το οποίο δεν είναι interpretable από μια εφαρμογή .NET.

##### <a name="bytesmessage"></a>BytesMessage

Ο ακόλουθος κώδικας δείχνει τον τρόπο για την εκμετάλλευση στο σώμα ενός αντικειμένου **BytesMessage** , χρησιμοποιώντας τα API .NET Bus υπηρεσίας.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

Ο ακόλουθος κώδικας δείχνει τον τρόπο για την εκμετάλλευση στο σώμα ενός αντικειμένου **MapMessage** , χρησιμοποιώντας τα API .NET Bus υπηρεσίας. Αυτός ο κωδικός επαναλαμβάνει τα στοιχεία του χάρτη, εμφανίζει το όνομα και την τιμή κάθε στοιχείου.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

Ο ακόλουθος κώδικας δείχνει τον τρόπο για την εκμετάλλευση στο σώμα ενός αντικειμένου **StreamMessage** , χρησιμοποιώντας τα API .NET Bus υπηρεσίας. Αυτός ο κωδικός παραθέτει κάθε ένα από τα στοιχεία από τη ροή, μαζί με τους τύπους.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

Ο ακόλουθος κώδικας δείχνει τον τρόπο για την εκμετάλλευση στο σώμα ενός αντικειμένου **TextMessage** , χρησιμοποιώντας τα API .NET Bus υπηρεσίας. Αυτός ο κώδικας εμφανίζει τη συμβολοσειρά κειμένου που περιέχονται στο σώμα του μηνύματος.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Υπηρεσία Bus .NET APIs σε JMS

Οι παρακάτω ενότητες εμφανίζουν πώς μια εφαρμογή .NET να δημιουργήσετε ένα μήνυμα που λαμβάνεται στο JMS σε κάθε έναν από τους διάφορους τύπους JMS στο μήνυμα. Παράδειγμα **ObjectMessage** δεν έχει περιλαμβάνονται, όπως το σώμα του ένα **ObjectMessage** περιέχει ένα αντικείμενο σειριοποιηθεί στο τη γλώσσα προγραμματισμού Java, το οποίο δεν είναι interpretable από μια εφαρμογή .NET.

##### <a name="bytesmessage"></a>BytesMessage

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε ένα αντικείμενο [BrokeredMessage][] στο .NET που λαμβάνεται από ένα πρόγραμμα-πελάτη JMS ως μια **BytesMessage**.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε ένα αντικείμενο [BrokeredMessage][] στο .NET που λαμβάνεται από ένα πρόγραμμα-πελάτη JMS ως μια **StreamMessage**.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

Ο ακόλουθος κώδικας δείχνει τον τρόπο εκμετάλλευση στο κυρίως σώμα του ένα **TextMessage** με χρήση του API υπηρεσίας Bus .NET. Αυτός ο κώδικας εμφανίζει τη συμβολοσειρά κειμένου που περιέχονται στο σώμα του μηνύματος.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Ιδιότητες εφαρμογής

####<a name="jms-to-service-bus-net-apis"></a>JMS για την εξυπηρέτηση Bus .NET APIs

Μηνύματα JMS υποστηρίζουν ιδιότητες εφαρμογής από τους παρακάτω τύπους: **boolean**, **byte**, **Σύντομη**, **int**, **μεγάλο**, **Αιώρηση**, **διπλά**και **συμβολοσειρά**. Ο ακόλουθος κώδικας Java δείχνει πώς μπορείτε να ορίσετε ιδιότητες για ένα μήνυμα με τη χρήση κάθε έναν από αυτούς τους τύπους ιδιοτήτων.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

Στο τα API .NET Bus υπηρεσίας, ιδιότητες εφαρμογής μήνυμα μεταφέρονται στη συλλογή **Properties** του [BrokeredMessage][]. Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να διαβάσετε τις ιδιότητες εφαρμογής ενός μηνύματος που λάβατε από ένα πρόγραμμα-πελάτη JMS.

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

Ο παρακάτω πίνακας δείχνει πώς οι τύποι ιδιοτήτων JMS αντιστοίχιση με τους τύπους ιδιοτήτων .NET.

| Τύπος ιδιότητας JMS | Τύπος ιδιότητας .NET |
|-------------------|--------------------|
| Byte              | SByte              |
| Ακέραιος αριθμός           | Int                |
| Αιώρηση             | Αιώρηση              |
| Δύο            | διπλά             |
| Δυαδική τιμή           | bool               |
| Συμβολοσειρά            | συμβολοσειρά             |

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

Ο ακόλουθος κώδικας Java δείχνει πώς μπορείτε να διαβάσετε τις ιδιότητες εφαρμογής ενός μηνύματος που λάβατε από ένα πρόγραμμα-πελάτη .NET Bus υπηρεσίας.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

Ο παρακάτω πίνακας δείχνει πώς οι τύποι ιδιοτήτων .NET αντιστοίχιση με τους τύπους ιδιοτήτων JMS.

| Τύπος ιδιότητας .NET | Τύπος ιδιότητας JMS | Σημειώσεις                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | UnsignedByte      | -                                                                                                                                                                      |
| SByte              | Byte              | -                                                                                                                                                                     |
| CHAR               | Χαρακτήρας         | -                                                                                                                                                                     |
| σύντομο              | Σύντομο             | -                                                                                                                                                                     |
| USHORT             | UnsignedShort     | -                                                                                                                                                                     |
| Int                | Ακέραιος αριθμός           | -                                                                                                                                                                     |
| UInt               | UnsignedInteger   | -                                                                                                                                                                     |
| μεγάλη               | Μεγάλη              | -                                                                                                                                                                     |
| ULONG              | UnsignedLong      | -                                                                                                                                                                     |
| Αιώρηση              | Αιώρηση             | -                                                                                                                                                                     |
| διπλά             | Δύο            | -                                                                                                                                                                     |
| δεκαδικών ψηφίων            | BigDecimal        | -                                                                                                                                                                     |
| bool               | Δυαδική τιμή           | -                                                                                                                                                                     |
| GUID               | UUID              | -                                                                                                                                                                     |
| συμβολοσειρά             | Συμβολοσειρά            | -                                                                                                                                                                     |
| Ημερομηνίας και ώρας           | Ημερομηνία              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | Αντιστοίχιση τύπο AMQP DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Χρονικό διάστημα           | DescribedType     | Αντιστοίχιση τύπο AMQP Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType     | Αντιστοίχιση τύπο AMQP Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Τυπική κεφαλίδες

Οι παρακάτω πίνακες δείχνουν πώς οι κεφαλίδες τυπική JMS και των τυπικών ιδιοτήτων [BrokeredMessage][] είναι αντιστοιχισμένα με χρήση AMQP 1.0.

#### <a name="jms-to-service-bus-net-apis"></a>JMS για την εξυπηρέτηση Bus .NET APIs

| JMS              | Υπηρεσία Bus .NET               | Σημειώσεις                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Δεν είναι διαθέσιμη αυτήν τη στιγμή        | Υπηρεσία Bus υποστηρίζει μόνο διαρκή μηνύματα. Για παράδειγμα, DeliveryMode.PERSISTENT, ανεξάρτητα από το τι έχει καθοριστεί.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Το μήνυμα. Το TimeToLive            | Μετατροπή                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Από προεπιλογή, JMSMessageID κωδικοποιείται σε δυαδική μορφή στο μήνυμα AMQP. Μετά τη λήψη της δυαδικό αναγνωριστικό μηνύματος, τη βιβλιοθήκη προγράμματος-πελάτη .NET μετατρέπει σε μια αναπαράσταση συμβολοσειράς με βάση τις τιμές unicode των byte. Για να μεταβείτε στη βιβλιοθήκη JMS για να χρησιμοποιήσετε αναγνωριστικά μηνυμάτων συμβολοσειρά, προσαρτήσετε το "δυαδικό-αναγνωριστικού μηνύματος = false" συμβολοσειρά οι παράμετροι ερωτήματος από το ConnectionURL JNDI. Για παράδειγμα: “amqps://[username]:[password]@[namespace].servicebus.windows.net? δυαδικό-αναγνωριστικού μηνύματος = false ". |
| JMSPriority      | Δεν είναι διαθέσιμη αυτήν τη στιγμή        | Υπηρεσία Bus δεν υποστηρίζει προτεραιότητα του μηνύματος.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Δεν είναι διαθέσιμη αυτήν τη στιγμή        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Το μήνυμα. Τη               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Μετατροπή                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms-τύπος"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Υπηρεσία Bus .NET APIs σε JMS

| Υπηρεσία Bus .NET        | JMS              | Σημειώσεις                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Δεν είναι διαθέσιμη αυτήν τη στιγμή |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Μετατροπή              |
| Ετικέτα                   | δ/υ              | Δεν είναι διαθέσιμη αυτήν τη στιγμή |
| Αναγνωριστικού μηνύματος               | JMSMessageID     | -                         |
| Τη                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | δ/υ              | Δεν είναι διαθέσιμη αυτήν τη στιγμή |
| ScheduledEnqueueTimeUtc | δ/υ              | Δεν είναι διαθέσιμη αυτήν τη στιγμή |
| Αναγνωριστικό περιόδου λειτουργίας               | δ/υ              | Δεν είναι διαθέσιμη αυτήν τη στιγμή |
| Το TimeToLive              | JMSExpiration    | Μετατροπή              |
| Για να                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Μη υποστηριζόμενες δυνατότητες και περιορισμούς

Κατά τη χρήση JMS μέσω AMQP 1.0 με υπηρεσία Bus, ισχύουν οι παρακάτω περιορισμοί:

-   Επιτρέπεται μόνο μία **MessageProducer** ή **MessageConsumer** ανά περίοδο λειτουργίας. Εάν θέλετε να δημιουργήσετε πολλά αντικείμενα **MessageProducer** ή **MessageConsumer** σε μια εφαρμογή, δημιουργήστε αποκλειστικό περιόδους λειτουργίας για κάθε μία από αυτές.

-   Δυναμικές θέμα συνδρομές δεν υποστηρίζονται αυτήν τη στιγμή.

-   Αντικείμενα **MessageSelector** δεν υποστηρίζονται.

-   Προσωρινό προορισμούς; Για παράδειγμα, **TemporaryQueue** ή **TemporaryTopic**, δεν υποστηρίζονται, μαζί με το **QueueRequestor** και **TopicRequestor** API που τους χρησιμοποιήσουν.

-   Περίοδοι λειτουργίας συναλλαγής δεν υποστηρίζονται.

-   Κατανεμημένες συναλλαγές δεν υποστηρίζονται.

## <a name="next-steps"></a>Επόμενα βήματα

Είστε έτοιμοι για να μάθετε περισσότερα; Επισκεφθείτε τις παρακάτω συνδέσεις:

- [Επισκόπηση AMQP Bus υπηρεσίας]
- [AMQP στην υπηρεσία Bus για τον Windows Server]

[AMQP στην υπηρεσία Bus για τον Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Επισκόπηση AMQP Bus υπηρεσίας]: service-bus-amqp-overview.md
[Πύλη του Azure]: https://portal.azure.com
