## <a name="send-messages-to-event-hubs"></a>Αποστολή μηνυμάτων με διανομείς συμβάν

Η βιβλιοθήκη προγράμματος-πελάτη Java για διανομείς συμβάν είναι διαθέσιμη για χρήση σε έργα Maven από το [Κεντρικό αποθετήριο Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)και μπορούν να χρησιμοποιηθούν με την ακόλουθη δήλωση εξάρτηση αρχείο έργου σας Maven:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Για διαφορετικούς τύπους Δόμηση περιβάλλοντα, μπορείτε να αποκτήσετε το πιο πρόσφατο τελική ΒΆΖΟ τα αρχεία ρητά από το [Κεντρικό αποθετήριο Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) ή από [το σημείο διανομής κυκλοφορίας στην GitHub](https://github.com/Azure/azure-event-hubs/releases).  

Για μια απλή συμβάν publisher, εισαγάγετε το πακέτο *com.microsoft.azure.eventhubs* για το συμβάν διανομείς τάξεις προγράμματος-πελάτη και του πακέτου *com.microsoft.azure.servicebus* για τις κατηγορίες βοηθητικού προγράμματος όπως συνήθεις εξαιρέσεις που είναι κοινόχρηστα με το πρόγραμμα-πελάτη ανταλλαγής μηνυμάτων Bus υπηρεσίας Azure. 

Για το παρακάτω παράδειγμα, πρώτα να δημιουργήσετε ένα νέο έργο Maven για μια εφαρμογή κονσόλας/κελύφους στο περιβάλλον ανάπτυξής σας αγαπημένες Java. Τάξη θα ονομάζεται ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Αντικαταστήστε το χώρο ονομάτων και ονομάτων διανομέα συμβάν με τις τιμές που χρησιμοποιούνται όταν δημιουργήσατε την ενότητα συμβάντων.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Στη συνέχεια, δημιουργήστε ένα συμβάν στον ενικό κατά τη μετατροπή μιας συμβολοσειράς σε την κωδικοποίηση UTF-8 byte. Στη συνέχεια, θα σας δημιουργήσετε μια νέα παρουσία διανομείς συμβάντων στο πρόγραμμα-πελάτη από τη συμβολοσειρά σύνδεσης και στείλτε το μήνυμα.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
