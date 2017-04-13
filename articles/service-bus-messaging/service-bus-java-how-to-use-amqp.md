<properties 
    pageTitle="Χρήση AMQP 1.0 με την υπηρεσία Bus Java API | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία μήνυμα Java (JMS) με το Azure Service Bus και ουρά μηνυμάτων για προχωρημένους"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"  
    manager="timlt" 
    editor="" />

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Πώς μπορείτε να χρησιμοποιήσετε σας το Java μήνυμα υπηρεσίας (JMS) API με υπηρεσία Bus και AMQP 1.0

Το σύνθετο μήνυμα ουράς πρωτοκόλλου (AMQP) 1.0 είναι μια αποτελεσματική, αξιόπιστη, γραμμικό επιπέδου ανταλλαγής μηνυμάτων πρωτόκολλο που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε ισχυρές, πλατφόρμες, εφαρμογών ανταλλαγής μηνυμάτων. Υποστήριξη AMQP 1.0 προστέθηκε Azure Service Bus στο Οκτώβριος 2012 και μεταπηδά σε γενικής διαθεσιμότητας (GA) στο Μαΐου 2013.

Η προσθήκη του AMQP 1.0 σημαίνει ότι τώρα είναι δυνατό να αξιοποιήσετε την υπηρεσία ουράς και δημοσίευση/εγγραφή όπου μεσολαβούν ανταλλαγής μηνυμάτων για τις δυνατότητες του Bus υπηρεσίας από μια ποικιλία πλατφόρμες χρησιμοποιώντας μια αποτελεσματική δυαδικό πρωτόκολλο. Επιπλέον, μπορείτε να δημιουργήσετε εφαρμογές που αποτελείται από τα στοιχεία που δημιουργούνται χρησιμοποιώντας συνδυασμό των γλωσσών, των πλαισίων και λειτουργικά συστήματα.

Αυτός ο οδηγός οδηγίες εξηγεί πώς να χρησιμοποιείτε την υπηρεσία Bus όπου μεσολαβούν ανταλλαγής μηνυμάτων δυνατότητες (ουρές και δημοσίευση/εγγραφή θέματα) από εφαρμογές Java χρησιμοποιώντας την τυπική δημοφιλείς Java μήνυμα υπηρεσίας (JMS) API.

## <a name="get-started-with-service-bus"></a>Γρήγορα αποτελέσματα με το Bus υπηρεσίας

Αυτός ο οδηγός προϋποθέτει ότι έχετε ήδη ένα χώρο ονομάτων Bus υπηρεσίας που περιέχει μια ουρά με το όνομα **ουρά queue1**. Εάν δεν το κάνετε, στη συνέχεια, μπορείτε να [δημιουργήσετε το χώρο ονομάτων και ουρά](service-bus-create-namespace-portal.md) με την [πύλη του Azure](https://portal.azure.com). Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε πεδία ονομάτων Bus υπηρεσίας και ουρές, δείτε [πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας](service-bus-dotnet-get-started-with-queues.md).

### <a name="downloading-the-amqp-10-jms-client-library"></a>Τη λήψη της βιβλιοθήκης AMQP 1.0 JMS προγράμματος-πελάτη

Για πληροφορίες σχετικά με τη θέση για να κάνετε λήψη την πιο πρόσφατη έκδοση της βιβλιοθήκης του προγράμματος-πελάτη Apache Qpid JMS AMQP 1.0, επισκεφθείτε [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Πρέπει να προσθέσετε τα ακόλουθα τέσσερα ΒΆΖΟ αρχεία από το αρχείο αρχειοθέτησης διανομής Apache Qpid JMS AMQP 1.0 το CLASSPATH Java όταν η δημιουργία και εκτέλεση εφαρμογών JMS με Bus υπηρεσίας:

*    geronimo jms\_1.1\_προδιαγραφή 1.0.jar
*    qpid-amqp-1-0-Client-[Version].jar
*    qpid-amqp-1-0-Client-jms-[Version].jar
*    qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Κωδικοποίηση εφαρμογών Java

### <a name="java-naming-and-directory-interface-jndi"></a>Ονομασία Java και καταλόγου περιβάλλοντος εργασίας (JNDI)

JMS χρησιμοποιεί την ονομασία Java και καταλόγου περιβάλλοντος εργασίας (JNDI) για να δημιουργήσετε ένα διαχωρισμό μεταξύ λογικά ονόματα και ονόματα των φυσικών. Δύο τύποι αντικειμένων JMS επιλύονται χρησιμοποιώντας JNDI: ConnectionFactory και προορισμού. JNDI χρησιμοποιεί ένα μοντέλο υπηρεσίας παροχής στο οποίο μπορείτε να συνδέσετε διάφορες υπηρεσίες καταλόγων για το χειρισμό των καθηκόντων ανάλυση ονόματος. Το Apache Qpid JMS AMQP 1.0 βιβλιοθήκη συνοδεύεται από ένα απλό ιδιότητες μορφοποίηση παροχής JNDI βασίζεται στο αρχείο που έχει ρυθμιστεί με τις ιδιότητες αρχείου από τα εξής:

```
# servicebus.properties – sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[username]:[password]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Ρύθμιση παραμέτρων του ConnectionFactory

Η εγγραφή χρησιμοποιείται για τον ορισμό ενός **ConnectionFactory** στην υπηρεσία παροχής JNDI αρχείο Qpid ιδιότητες είναι την εξής μορφή:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Όπου **[jndi_name]** και **[ConnectionURL]** έχουν την εξής σημασία:

- **[jndi_name]**: το λογικό όνομα της το ConnectionFactory. Αυτό είναι το όνομα που θα επιλυθούν στην εφαρμογή Java χρησιμοποιώντας τη μέθοδο JNDI IntialContext.lookup().
- **[ConnectionURL]**: μια διεύθυνση URL που παρέχει τη βιβλιοθήκη JMS με τις πληροφορίες που απαιτούνται για το broker AMQP.

Η μορφή για το **ConnectionURL** είναι ως εξής:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Όταν **[χώρος ονομάτων]**, **[όνομα χρήστη]** και **[κωδικός πρόσβασης]** έχουν την εξής σημασία:

- **[χώρος ονομάτων]**: Η υπηρεσία Bus χώρο ονομάτων.
- **[όνομα χρήστη]**: Η υπηρεσία Bus όνομα εκδότη.
- **[κωδικός πρόσβασης]**: κωδικοποίηση URL φόρμας του αριθμού-κλειδιού υπηρεσίας Bus εκδότη.

> [AZURE.NOTE] Πρέπει να κωδικοποιήσετε διεύθυνση URL του κωδικού πρόσβασης με μη αυτόματο τρόπο. Ένα χρήσιμο βοηθητικό πρόγραμμα κωδικοποίηση URL είναι διαθέσιμη από την [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Ρύθμιση παραμέτρων των προορισμών

Η εγγραφή χρησιμοποιείται για τον ορισμό ενός προορισμού στην υπηρεσία παροχής JNDI αρχείο Qpid ιδιότητες είναι την εξής μορφή:

```
queue.[jndi_name] = [physical_name]
```
ή

```
topic.[jndi_name] = [physical_name]
```

Όπου **[jndi\_όνομα]** και **[φυσικής\_όνομα]** έχουν την εξής σημασία:

- **[jndi_name]**: το λογικό όνομα του προορισμού. Αυτό είναι το όνομα που θα επιλυθούν στην εφαρμογή Java χρησιμοποιώντας τη μέθοδο JNDI IntialContext.lookup().
- **[physical_name]**: το όνομα της οντότητας Bus υπηρεσία στην οποία η εφαρμογή αποστέλλει ή λαμβάνει μηνύματα.

> [AZURE.NOTE] Κατά τη λήψη από μια συνδρομή θέμα Bus υπηρεσία, το φυσικό όνομα που καθορίζεται στο JNDI πρέπει να είναι το όνομα του θέματος. Όταν δημιουργείται η συνδρομή διαρκή στον κώδικα εφαρμογής JMS παρέχονται στο όνομα της συνδρομής. Ο [Οδηγός 1.0 AMQP Bus υπηρεσίας για προγραμματιστές](service-bus-amqp-dotnet.md) παρέχει περισσότερες λεπτομέρειες σχετικά με την εργασία με υπηρεσία Bus θέμα συνδρομές από JMS.

### <a name="write-the-jms-application"></a>Γράψτε την εφαρμογή JMS

Υπάρχουν χωρίς ειδική API ή επιλογές απαιτούνται κατά τη χρήση JMS με Bus υπηρεσίας. Ωστόσο, υπάρχουν μερικά περιορισμούς που θα καλύπτονται αργότερα. Όπως με οποιαδήποτε εφαρμογή του JMS, τα πρώτα πρώτον απαιτούμενα ρύθμισης παραμέτρων του περιβάλλοντος JNDI, για να μπορέσετε να επιλύσετε μια **ConnectionFactory** και τους προορισμούς.

#### <a name="configure-the-jndi-initialcontext"></a>Ρύθμιση παραμέτρων του InitialContext JNDI

Το περιβάλλον JNDI έχει ρυθμιστεί, μεταφέροντας μια hashtable των πληροφοριών ρύθμισης παραμέτρων στην κατασκευή της κλάσης javax.naming.InitialContext. Δύο απαιτείται στοιχεία του hashtable είναι το όνομα της κλάσης από το αρχικό περιβάλλον εργοστασίου και τη διεύθυνση URL της υπηρεσίας παροχής. Τον ακόλουθο κώδικα παρουσιάζει πώς μπορείτε να ρυθμίσετε τις παραμέτρους του περιβάλλοντος JNDI για να χρησιμοποιήσετε το αρχείο ιδιότητες Qpid βάσει JNDI παροχής με ένα αρχείο ιδιότητες που ονομάζεται **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Μια απλή εφαρμογή JMS χρησιμοποιώντας μια ουρά Bus υπηρεσίας

Το παρακάτω παράδειγμα πρόγραμμα αποστέλλει JMS TextMessages σε μια υπηρεσία Bus ουρά με το όνομα λογικές JNDI ουράς και λαμβάνει ξανά τα μηνύματα.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Lookup ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Εκτέλεση της εφαρμογής παράγει το εξής αποτέλεσμα:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Πλατφόρμες μηνυμάτων μεταξύ JMS και .NET

Αυτός ο οδηγός εμφάνιζε πώς μπορείτε να στέλνετε και λαμβάνετε μηνύματα από Bus υπηρεσίας και χρήση JMS. Ωστόσο, ένα από τα βασικά πλεονεκτήματα του AMQP 1.0 είναι ότι το επιτρέπει σε εφαρμογές να δημιουργηθεί από στοιχεία γραμμένο σε διαφορετικές γλώσσες, με μηνυμάτων που ανταλλάσσονται με αξιοπιστία όσο και στην πλήρη πιστότητα.

Χρησιμοποιώντας το δείγμα εφαρμογής JMS που περιγράφεται παραπάνω και μια παρόμοια εφαρμογή .NET που λαμβάνονται από έναν οδηγό συνοδευτικό, [πώς μπορείτε να χρησιμοποιήσετε 1.0 AMQP με το API της υπηρεσίας Bus .NET το .NET](service-bus-dotnet-advanced-message-queuing.md), μπορείτε να ανταλλάξετε μηνύματα μεταξύ .NET και Java. 

Για περισσότερες πληροφορίες σχετικά με τις λεπτομέρειες της πλατφόρμες μηνυμάτων χρησιμοποιώντας Bus υπηρεσίας και AMQP 1.0, ανατρέξτε στο θέμα [υπηρεσία Bus AMQP 1.0 για προγραμματιστές του οδηγού](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS για .NET

Για μια επίδειξη JMS .NET ανταλλαγής μηνυμάτων:

* Εκκινήστε την εφαρμογή δείγμα .NET χωρίς κανένα όρισμα γραμμής εντολών.
* Ξεκινήστε το δείγμα εφαρμογής Java με το όρισμα γραμμής εντολών "sendonly". Σε αυτήν τη λειτουργία, η εφαρμογή δεν θα λαμβάνετε μηνύματα από την ουρά, θα στείλει μόνο.
* Πατήστε το πλήκτρο **Enter** μερικές φορές στην κονσόλα εφαρμογή Java, γεγονός που θα κάνει αποστολή μηνυμάτων.
* Αυτά τα μηνύματα λαμβάνονται από την εφαρμογή .NET.

#### <a name="output-from-jms-application"></a>Έξοδος από την εφαρμογή JMS

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Έξοδος από την εφαρμογή .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET σε JMS

Για μια επίδειξη του .NET JMS ανταλλαγής μηνυμάτων:

* Ξεκινήστε το δείγμα εφαρμογής .NET με το όρισμα γραμμής εντολών "sendonly". Σε αυτήν τη λειτουργία, η εφαρμογή δεν θα λαμβάνετε μηνύματα από την ουρά, θα στείλει μόνο.
* Ξεκινήστε το δείγμα εφαρμογής Java χωρίς κανένα όρισμα γραμμής εντολών.
* Πατήστε το πλήκτρο **Enter** μερικές φορές στην κονσόλα εφαρμογή .NET, που θα προκαλέσει την αποστολή μηνυμάτων.
* Αυτά τα μηνύματα λαμβάνονται από την εφαρμογή Java.

#### <a name="output-from-net-application"></a>Έξοδος από την εφαρμογή .NET

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Έξοδος από την εφαρμογή JMS

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Μη υποστηριζόμενες δυνατότητες και περιορισμούς

Οι παρακάτω περιορισμοί ισχύουν κατά τη χρήση JMS μέσω AMQP 1.0 με Bus υπηρεσίας, δηλαδή:

* Επιτρέπεται μόνο μία **MessageProducer** ή **MessageConsumer** ανά **περίοδο λειτουργίας**. Εάν χρειάζεστε για να δημιουργήσετε πολλές **MessageProducers** ή **MessageConsumers** σε μια εφαρμογή, δημιουργήστε ένα αποκλειστικό **περιόδου λειτουργίας** για κάθε μία από αυτές.
* Δυναμικές θέμα συνδρομές δεν υποστηρίζονται αυτήν τη στιγμή.
* **MessageSelectors** δεν υποστηρίζονται αυτήν τη στιγμή.
* Προσωρινό προορισμούς, δηλαδή, **TemporaryQueue**, **TemporaryTopic** δεν αυτήν τη στιγμή υποστηρίζονται, μαζί με το **QueueRequestor** και **TopicRequestor** API που τους χρησιμοποιήσουν.
* Περίοδοι λειτουργίας συναλλαγής και κατανεμημένες συναλλαγές δεν υποστηρίζονται.

## <a name="summary"></a>Σύνοψη

Σε αυτό το άρθρο εμφάνιζε πώς μπορείτε να χρησιμοποιήσετε Bus υπηρεσία ανταλλαγής μηνυμάτων δυνατότητες (ουρές και δημοσίευση/εγγραφή θέματα) από Java χρησιμοποιώντας το δημοφιλείς JMS API και AMQP 1.0.

Μπορείτε επίσης να χρησιμοποιήσετε την υπηρεσία Bus AMQP 1.0 από άλλες γλώσσες, συμπεριλαμβανομένων των .NET, C, Python και PHP. Στοιχεία που δημιουργούνται χρησιμοποιώντας αυτές τις διαφορετικές γλώσσες να αξιόπιστα ανταλλαγή μηνυμάτων και στην πλήρη πιστότητα χρησιμοποιώντας το AMQP 1.0 υποστηρίζει σε υπηρεσία Bus. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [υπηρεσία Bus AMQP 1.0 για προγραμματιστές του οδηγού](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Επόμενα βήματα

* [Υποστήριξη AMQP 1.0 στο Bus υπηρεσίας Azure](service-bus-amqp-overview.md)
* [Πώς μπορείτε να χρησιμοποιήσετε AMQP 1.0 με το API υπηρεσίας Bus .NET](service-bus-dotnet-advanced-message-queuing.md)
* [Οδηγός 1.0 AMQP Bus υπηρεσίας για προγραμματιστές](service-bus-amqp-dotnet.md)
* [Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας](service-bus-dotnet-get-started-with-queues.md)
 
