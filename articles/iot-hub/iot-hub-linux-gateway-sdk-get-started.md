<properties
    pageTitle="Γρήγορα αποτελέσματα με το SDK IoT διανομέα πύλης | Microsoft Azure"
    description="Αυτόν τον Οδηγό Azure IoT πύλης SDK χρησιμοποιεί Linux για την απεικόνιση βασικές έννοιες, θα πρέπει να γνωρίζετε κατά τη χρήση του SDK Azure IoT πύλης."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>SDK πύλης Azure IoT (beta) - γρήγορα αποτελέσματα με το Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Πώς να δημιουργείτε το δείγμα

Πριν ξεκινήσετε, πρέπει να [ρυθμίσετε το περιβάλλον ανάπτυξης] [ lnk-setupdevbox] για την εργασία με το SDK στην Linux.

1. Ανοίξτε ένα κέλυφος.
2. Μεταβείτε στον ριζικό φάκελο στο τοπικό αντίγραφο του αποθετηρίου **azure iot-πύλης sdk** .
3. Εκτελέστε τη δέσμη ενεργειών **tools/build.sh** . Αυτή η δέσμη ενεργειών χρησιμοποιεί το βοηθητικό πρόγραμμα **cmake** για να δημιουργήσετε ένα φάκελο που ονομάζεται **Δημιουργία** στον ριζικό φάκελο του τοπικού αντιγράφου του αποθετηρίου **azure iot-πύλης sdk** και δημιουργούν ένα αρχείο κατασκευής. Η δέσμη ενεργειών, στη συνέχεια, δημιουργεί τη λύση και εκτελεί τις δοκιμές.

> [AZURE.NOTE]  Κάθε φορά που εκτελείτε τη δέσμη ενεργειών **build.sh** , διαγράφει και, στη συνέχεια, δημιουργεί εκ νέου στο φάκελο που **δημιουργείτε** στο ριζικό φάκελο του τοπικού αντιγράφου του αποθετηρίου **azure-iot-πύλης-sdk** .

## <a name="how-to-run-the-sample"></a>Πώς μπορείτε να εκτελέσετε το δείγμα

1. Η δέσμη ενεργειών **build.sh** δημιουργεί τα αποτελέσματά στο φάκελο " **Δημιουργία** " στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** . Περιλαμβάνονται και οι δύο λειτουργικές μονάδες που χρησιμοποιούνται σε αυτό το δείγμα.

    Η δέσμη ενεργειών δόμησης τοποθετεί **liblogger_hl.so** στο το **Δόμηση/λειτουργικές μονάδες καταγραφής/** φάκελο και **libhello_world_hl.so** στο το **λειτουργικές μονάδες/Δόμηση/hello_world/** φακέλου. Χρησιμοποιήστε αυτές τις διαδρομές για την τιμή της **λειτουργικής μονάδας διαδρομής** , όπως φαίνεται στο παρακάτω αρχείο ρυθμίσεων JSON.

2. Το αρχείο **hello_world_lin.json** στο φάκελο **δείγματα/hello_world src** είναι ένα παράδειγμα αρχείου ρυθμίσεις JSON για Linux που μπορείτε να χρησιμοποιήσετε για να εκτελέσετε το δείγμα. Οι ρυθμίσεις JSON παράδειγμα που φαίνεται παρακάτω προϋποθέτει ότι θα μπορείτε να εκτελέσετε το δείγμα από το ριζικό κατάλογο της του τοπικού αντιγράφου του αποθετηρίου **azure-iot-πύλης-sdk** .

3. Για τη λειτουργική μονάδα **logger_hl** , στην ενότητα **ορισμάτων** , ορίστε την τιμή **όνομα αρχείου** για το όνομα και τη διαδρομή του αρχείου που θα περιέχει τα δεδομένα του αρχείου καταγραφής.

    Αυτό είναι ένα παράδειγμα ενός αρχείου ρυθμίσεις JSON για Linux που θα εγγραφεί στο το **log.txt** στο φάκελο όπου μπορείτε να εκτελέσετε το δείγμα.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Στο κέλυφος σας, μεταβείτε σε φάκελο **azure-iot-πύλης-sdk** .
4. Εκτελέστε την ακόλουθη εντολή:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
