<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε AMQP 1.0 με το API Bus υπηρεσία .NET | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε για προχωρημένους μήνυμα ουράς Protodol (AMQP) 1.0 με το API Bus Azure .NET υπηρεσίας." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Πώς μπορείτε να χρησιμοποιήσετε AMQP 1.0 με το API υπηρεσίας Bus .NET

Το σύνθετο μήνυμα ουράς πρωτοκόλλου (AMQP) 1.0 είναι μια αποτελεσματική, αξιόπιστη, γραμμικό επιπέδου ανταλλαγής μηνυμάτων πρωτόκολλο που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε ισχυρές, πλατφόρμες, εφαρμογών ανταλλαγής μηνυμάτων.

Υποστήριξη για 1.0 AMQP στην υπηρεσία Bus σημαίνει ότι μπορείτε να χρησιμοποιήσετε την υπηρεσία ουράς και δημοσίευση/εγγραφή όπου μεσολαβούν δυνατότητες ανταλλαγής μηνυμάτων από μια ποικιλία πλατφόρμες χρησιμοποιώντας μια αποτελεσματική δυαδικό πρωτόκολλο. Επιπλέον, μπορείτε να δημιουργήσετε εφαρμογές που αποτελείται από τα στοιχεία που δημιουργούνται χρησιμοποιώντας συνδυασμό των γλωσσών, των πλαισίων και λειτουργικά συστήματα.

Σε αυτό το άρθρο εξηγεί πώς να χρησιμοποιείτε την υπηρεσία Bus όπου μεσολαβούν ανταλλαγής μηνυμάτων δυνατότητες (ουρές και θέματα για δημοσίευση/εγγραφή) από τις εφαρμογές .NET με χρήση του API .NET Bus υπηρεσίας. Υπάρχει ένα [άρθρο συνοδευτικό](service-bus-java-how-to-use-jms-api-amqp.md) που εξηγεί πώς μπορείτε να κάνετε το ίδιο χρησιμοποιώντας την τυπική API υπηρεσίας μήνυμα Java (JMS). Μπορείτε να χρησιμοποιήσετε αυτές τις δύο οδηγοί μαζί για να μάθετε περισσότερα σχετικά με την πλατφόρμα μηνυμάτων με χρήση AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Γρήγορα αποτελέσματα με το Bus υπηρεσίας

Σε αυτό το άρθρο προϋποθέτει ότι έχετε ήδη ένα χώρο ονομάτων Bus υπηρεσίας που περιέχει μια ουρά με το όνομα "ουρά queue1." Εάν δεν το κάνετε, στη συνέχεια, μπορείτε να δημιουργήσετε το χώρο ονομάτων και ουρά με την [πύλη του Azure][]. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε πεδία ονομάτων Bus υπηρεσίας και ουρές, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το ουρές Bus υπηρεσίας](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Κάντε λήψη του Bus υπηρεσίας SDK

Υποστήριξη AMQP 1.0 είναι διαθέσιμη στην υπηρεσία Bus SDK έκδοση 2.1 ή νεότερη έκδοση. Μπορείτε να λάβετε τα πιο πρόσφατα bit από NuGet στο [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Εφαρμογές .NET κώδικα

Από προεπιλογή, τη βιβλιοθήκη προγράμματος-πελάτη υπηρεσίας Bus .NET επικοινωνεί με την υπηρεσία Bus υπηρεσία χρησιμοποιώντας ένα αποκλειστικό πρωτόκολλο SOAP βάσει. Για να χρησιμοποιήσετε AMQP 1.0 αντί για την προεπιλεγμένη πρωτόκολλο απαιτεί ρητή ρύθμιση παραμέτρων στη συμβολοσειρά σύνδεσης υπηρεσίας Bus όπως περιγράφεται στην επόμενη ενότητα. Εκτός από αυτήν την αλλαγή, κώδικα της εφαρμογής παραμένει αμετάβλητη στην ουσία κατά τη χρήση AMQP 1.0.

Στην τρέχουσα έκδοση, υπάρχουν μερικές δυνατότητες API που δεν υποστηρίζονται κατά τη χρήση AMQP. Αυτές οι δυνατότητες που δεν υποστηρίζονται παρατίθενται αργότερα στην ενότητα [μη υποστηριζόμενες δυνατότητες και περιορισμούς](#unsupported-features-and-restrictions). Ορισμένες από τις ρυθμίσεις για προχωρημένους παραμέτρων έχουν διαφορετική έννοια επίσης κατά τη χρήση AMQP. Καμία από αυτές τις ρυθμίσεις που χρησιμοποιούνται σε αυτό το άρθρο, αλλά είναι διαθέσιμες στην [υπηρεσία Bus AMQP Επισκόπηση](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences)περισσότερες λεπτομέρειες.

### <a name="configure-via-appconfig"></a>Ρύθμιση παραμέτρων μέσω App.config

Είναι συνιστάται ότι εφαρμογές χρησιμοποιούν το αρχείο ρύθμισης παραμέτρων App.config για την αποθήκευση ρυθμίσεων. Για τις εφαρμογές υπηρεσίας Bus, μπορείτε να χρησιμοποιήσετε App.config για να αποθηκεύσετε την υπηρεσία Bus **συμβολοσειρά σύνδεσης**. Αυτού του δείγματος εφαρμογής χρησιμοποιεί επίσης App.config για να αποθηκεύσετε το όνομα του την υπηρεσία Bus οντότητα που χρησιμοποιεί την ανταλλαγή μηνυμάτων.

Ένα δείγμα αρχείου App.config εμφανίζεται εδώ:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Ρύθμιση παραμέτρων τη συμβολοσειρά σύνδεσης Bus υπηρεσίας

Η τιμή της ρύθμισης **Microsoft.ServiceBus.ConnectionString** είναι συμβολοσειρά σύνδεσης υπηρεσίας Bus που χρησιμοποιείται για τη ρύθμιση παραμέτρων της σύνδεσης με την υπηρεσία Bus. Η μορφή είναι ως εξής:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Όπου `[namespace]` και `[SAS key]` λαμβάνονται από την [πύλη του Azure][]. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας] [].

Όταν χρησιμοποιείτε AMQP, τη συμβολοσειρά σύνδεσης προσαρτάται με `;TransportType=Amqp`, που σας ενημερώνει για τη βιβλιοθήκη προγράμματος-πελάτη για να κάνετε τη σύνδεση σε υπηρεσία Bus χρησιμοποιώντας AMQP 1.0.

### <a name="configure-the-entity-name"></a>Ρύθμιση παραμέτρων το όνομα οντότητα

Αυτή η εφαρμογή δείγμα χρησιμοποιεί το `EntityName` ρύθμιση στην ενότητα **appSettings** του αρχείου App.config για να ρυθμίσετε το όνομα της ουράς με την οποία η εφαρμογή αντικαταστήσει μηνύματα.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Μια απλή εφαρμογή .NET χρησιμοποιώντας μια ουρά Bus υπηρεσίας

Το παρακάτω παράδειγμα στέλνει και λαμβάνει μηνύματα από μια υπηρεσία Bus ουρά και.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Εκτέλεση της εφαρμογής παράγει αποτέλεσμα της φόρμας:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Πλατφόρμες μηνυμάτων μεταξύ JMS και .NET

Αυτό το θέμα εμφάνιζε πώς μπορείτε να στείλετε μηνύματα σε υπηρεσία Bus χρησιμοποιώντας .NET, καθώς και πώς μπορείτε να λαμβάνετε αυτά τα μηνύματα χρησιμοποιώντας το .NET. Ωστόσο, ένα από τα βασικά πλεονεκτήματα του AMQP 1.0 είναι ότι το επιτρέπει σε εφαρμογές να δημιουργηθεί από στοιχεία γραμμένο σε διαφορετικές γλώσσες, με μηνυμάτων που ανταλλάσσονται με αξιοπιστία όσο και στην πλήρη πιστότητα.

Χρησιμοποιώντας το δείγμα εφαρμογής .NET που περιγράφεται παραπάνω και μια παρόμοια εφαρμογή Java που λαμβάνονται από έναν οδηγό συνοδευτικό, [πώς μπορείτε να χρησιμοποιήσετε σας το Java μήνυμα υπηρεσίας (JMS) API με υπηρεσία Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md), είναι δυνατή η ανταλλαγή μηνυμάτων μεταξύ .NET και Java. 

Για περισσότερες πληροφορίες σχετικά με τις λεπτομέρειες της πλατφόρμες μηνυμάτων χρησιμοποιώντας Bus υπηρεσίας και AMQP 1.0, ανατρέξτε στο θέμα η [Επισκόπηση υπηρεσίας Bus AMQP 1.0](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS για .NET

Για μια επίδειξη JMS .NET ανταλλαγής μηνυμάτων:

* Εκκινήστε την εφαρμογή δείγμα .NET χωρίς κανένα όρισμα γραμμής εντολών.
* Ξεκινήστε το δείγμα εφαρμογής Java με το όρισμα γραμμής εντολών "sendonly". Σε αυτήν τη λειτουργία, η εφαρμογή δεν θα λαμβάνετε μηνύματα από την ουρά, θα στείλει μόνο.
* Πατήστε το πλήκτρο **Enter** μερικές φορές στην κονσόλα εφαρμογή Java, γεγονός που θα κάνει αποστολή μηνυμάτων.
* Αυτά τα μηνύματα λαμβάνονται από την εφαρμογή .NET.

### <a name="output-from-jms-application"></a>Έξοδος από την εφαρμογή JMS

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Έξοδος από την εφαρμογή .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET σε JMS

Για μια επίδειξη του .NET JMS ανταλλαγής μηνυμάτων:

* Ξεκινήστε το δείγμα εφαρμογής .NET με το όρισμα γραμμής εντολών "sendonly". Σε αυτήν τη λειτουργία, η εφαρμογή δεν θα λαμβάνετε μηνύματα από την ουρά, θα στείλει μόνο.
* Ξεκινήστε το δείγμα εφαρμογής Java χωρίς κάποια ορίσματα γραμμής εντολών.
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

Οι παρακάτω δυνατότητες του API Bus υπηρεσία .NET δεν υποστηρίζονται αυτήν τη στιγμή κατά τη χρήση AMQP:

 * Συναλλαγές
 * Αποστολή μέσω μεταφοράς προορισμού

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [δυνατότητες που δεν υποστηρίζονται, περιορισμούς, και συμπεριφοράς διαφορές](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Σύνοψη

Σε αυτό το άρθρο εμφάνιζε πώς μπορείτε να έχετε πρόσβαση στις λειτουργίες ανταλλαγής μηνυμάτων όπου μεσολαβούν Bus υπηρεσίας (ουρές και δημοσίευση/εγγραφή θέματα) από το .NET χρησιμοποιώντας AMQP 1.0 και το API .NET Bus υπηρεσίας.

Μπορείτε επίσης να χρησιμοποιήσετε την υπηρεσία Bus AMQP 1.0 από άλλες γλώσσες όπως Java, C, Python και PHP. Στοιχεία που δημιουργούνται χρησιμοποιώντας αυτές τις γλώσσες να ανταλλαγή μηνυμάτων στην πλήρη πιστότητα χρησιμοποιώντας AMQP 1.0 στην υπηρεσία Bus και αξιόπιστα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα η [Επισκόπηση AMQP Bus υπηρεσίας](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε διαβάσει μια επισκόπηση των υπηρεσιών Bus και AMQP με .NET, ανατρέξτε στις παρακάτω συνδέσεις για περισσότερες πληροφορίες.

* [Υποστήριξη AMQP 1.0 στο Bus υπηρεσίας Azure](service-bus-amqp-overview.md)
* [Πώς μπορείτε να χρησιμοποιήσετε σας το Java μήνυμα υπηρεσίας (JMS) API με υπηρεσία Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας](service-bus-dotnet-get-started-with-queues.md)
 
[Πύλη του Azure]: https://portal.azure.com
