## <a name="send-messages-to-event-hubs"></a>Αποστολή μηνυμάτων με διανομείς συμβάντος

Σε αυτήν την ενότητα, θα σας θα εγγραφεί μια εφαρμογή C για να στείλετε τα συμβάντα σας διανομέα συμβάν. Θα χρησιμοποιήσουμε τη βιβλιοθήκη Proton AMQP από το [project Apache Qpid](http://qpid.apache.org/). Αυτό είναι ανάλογος για χρήση της υπηρεσίας Bus ουρές και θέματα με AMQP από το C όπως φαίνεται [εδώ](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Για περισσότερες πληροφορίες, ανατρέξτε [στην τεκμηρίωση Qpid Proton](http://qpid.apache.org/proton/index.html).

1. Από τη [σελίδα Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html), κάντε κλικ στη σύνδεση **Qpid Proton κατά την εγκατάσταση** και ακολουθήστε τις οδηγίες ανάλογα με το περιβάλλον σας. Θα υποθέσουμε ότι ένα περιβάλλον Linux. Για παράδειγμα, μια [Εικονική Linux Azure](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) με Ubuntu 14.04.

2. Για να μεταγλωττίσετε στη βιβλιοθήκη Proton, εγκαταστήστε τα ακόλουθα πακέτα:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Κάντε λήψη της βιβλιοθήκης [Qpid Proton βιβλιοθήκη](http://qpid.apache.org/proton/index.html) και εξαγωγή, π.χ.:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Δημιουργία ενός καταλόγου Δόμηση, μεταγλώττισης και εγκατάσταση:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. Στον κατάλογο εργασίας, δημιουργήστε ένα νέο αρχείο που ονομάζεται **sender.c** με το παρακάτω περιεχόμενο. Μην ξεχάσετε να αντικαταστήσετε την τιμή για το όνομα διανομέας συμβάντων και όνομα χώρου ονομάτων (η τελευταία είναι συνήθως `{event hub name}-ns`). Επίσης, θα πρέπει να αντικαταστήσετε μια έκδοση κωδικοποιημένη διεύθυνση URL του αριθμού-κλειδιού για το **SendRule** που δημιουργήσατε νωρίτερα. Μπορείτε να κωδικοποιήσετε διεύθυνση URL του [εδώ](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Μεταγλώττιση του αρχείου, υπό την προϋπόθεση ότι **πρόγραμμα μεταγλώττισης gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] Σε αυτόν τον κωδικό, μπορούμε να χρησιμοποιήσουμε ένα παράθυρο εξερχόμενων 1 για να επιβάλετε τα μηνύματα από το συντομότερο δυνατό. Σε γενικές γραμμές, η εφαρμογή σας θα πρέπει να προσπαθήστε να δέσμη μηνυμάτων για να αυξήσετε την ταχύτητα μετάδοσης. Ανατρέξτε στο θέμα [Qpid AMQP Messenger σελίδας](http://qpid.apache.org/components/messenger/index.html) για περισσότερες πληροφορίες σχετικά με το πώς μπορείτε να χρησιμοποιήσετε τη βιβλιοθήκη Qpid Proton σε αυτό και άλλα περιβάλλοντα και από πλατφόρμες για το οποίο παρέχονται συνδέσεις (προς το παρόν Perl, PHP, Python και φωνητικής γραφής).
