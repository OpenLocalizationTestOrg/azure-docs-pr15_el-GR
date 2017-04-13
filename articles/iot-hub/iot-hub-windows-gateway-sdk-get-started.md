<properties
    pageTitle="Γρήγορα αποτελέσματα με το SDK IoT διανομέα πύλης | Microsoft Azure"
    description="Για την απεικόνιση βασικές έννοιες, θα πρέπει να κατανοήσετε όταν χρησιμοποιείτε το SDK Azure IoT πύλης χρησιμοποιώντας το Windows Azure οδηγίες IoT πύλης SDK."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>SDK πύλης Azure IoT (beta) - να ξεκινήσετε να χρησιμοποιείτε τα Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Πώς να δημιουργείτε το δείγμα

Πριν ξεκινήσετε, πρέπει να [ρυθμίσετε το περιβάλλον ανάπτυξης] [ lnk-setupdevbox] για την εργασία με το SDK των Windows.

1. Ανοίξτε μια γραμμή εντολών **για προγραμματιστές εντολών για VS2015** .
2. Μεταβείτε στον ριζικό φάκελο στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** .
3. Εκτελέστε το **Εργαλεία\\build.cmd** δέσμης ενεργειών. Αυτή η δέσμη ενεργειών δημιουργεί ένα αρχείο λύση Visual Studio, δημιουργεί τη λύση και εκτελεί τις δοκιμές. Μπορείτε να βρείτε τη λύση Visual Studio στο φάκελο **Δόμηση** στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** .

## <a name="how-to-run-the-sample"></a>Πώς μπορείτε να εκτελέσετε το δείγμα

1. Η δέσμη ενεργειών **build.cmd** δημιουργεί ένα φάκελο που ονομάζεται **Δημιουργία** στο τοπικό αντίγραφο του αποθετηρίου. Αυτός ο φάκελος περιέχει τις δύο λειτουργικές μονάδες που χρησιμοποιούνται σε αυτό το δείγμα.

    Η δέσμη ενεργειών δόμησης τοποθετεί **logger_hl.dll** στο το **Δημιουργία\\λειτουργικές μονάδες\\καταγραφής\\εντοπισμός σφαλμάτων** φάκελο και **hello_world_hl.dll** στο το **Δημιουργία\\λειτουργικές μονάδες\\hello_world\\εντοπισμός σφαλμάτων** φακέλου. Χρησιμοποιήστε αυτές τις διαδρομές για την τιμή της **λειτουργικής μονάδας διαδρομής** , όπως φαίνεται στο παρακάτω αρχείο ρυθμίσεων JSON.

2. Το αρχείο **hello_world_win.json** στο το **δείγματα\\hello_world\\src** φάκελος είναι ένα παράδειγμα αρχείου ρυθμίσεις JSON για Windows που μπορείτε να χρησιμοποιήσετε για να εκτελέσετε το δείγμα. Οι ρυθμίσεις JSON παράδειγμα που φαίνεται παρακάτω προϋποθέτει ότι κλωνοποιηθεί του αποθετηρίου IoT πύλης SDK στη ρίζα της μονάδας δίσκου **C:** . Εάν έχετε κάνει λήψη του σε άλλη θέση, πρέπει να προσαρμόσετε τις τιμές **λειτουργική μονάδα διαδρομή** του **δείγματα\\hello_world\\src\\hello_world_win.json** αρχείων αντίστοιχα.

3. Για τη λειτουργική μονάδα **logger_hl** , στην ενότητα **ορισμάτων** , ορίστε την τιμή **όνομα αρχείου** για το όνομα και τη διαδρομή του αρχείου που θα περιέχει τα δεδομένα του αρχείου καταγραφής.

    Αυτό είναι ένα παράδειγμα ενός αρχείου JSON ρυθμίσεις των Windows που θα εγγραφής στο αρχείο **log.txt** στον ριζικό κατάλογο της μονάδας δίσκου **C:** .

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. Σε μια γραμμή εντολών, μεταβείτε στον ριζικό φάκελο του τοπικού αντιγράφου του αποθετηρίου **azure-iot-πύλης-sdk** .
4. Εκτελέστε την ακόλουθη εντολή:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md