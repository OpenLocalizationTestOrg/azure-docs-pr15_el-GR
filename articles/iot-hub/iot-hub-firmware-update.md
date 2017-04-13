<properties
 pageTitle="Πώς μπορείτε να κάνετε μια ενημερωμένη έκδοση υλικολογισμικού | Microsoft Azure"
 description="Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να κάνετε μια ενημερωμένη έκδοση υλικολογισμικού"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Πρόγραμμα εκμάθησης: Πώς μπορείτε να κάνετε μια υλικολογισμικού ενημέρωση (έκδοση preview)

## <a name="introduction"></a>Εισαγωγή
[Γρήγορα αποτελέσματα με τη Διαχείριση συσκευών] του[ lnk-dm-getstarted] πρόγραμμα εκμάθησης, είδατε πώς μπορείτε να χρησιμοποιήσετε τη [συσκευή διπλού] [ lnk-devtwin] και [cloud-σε-συσκευή μεθόδους (C2D)] [ lnk-c2dmethod] στοιχειώδεις τύπους απομακρυσμένα επανεκκίνηση μια συσκευή. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί το ίδιο στοιχειώδεις τύπους IoT διανομέας και παρέχει οδηγίες και σας δείχνει πώς μπορείτε να κάνετε μια ενημερωμένη έκδοση για να ολοκληρωμένες προσομοιωμένη υλικολογισμικού.  Αυτό το μοτίβο χρησιμοποιείται κατά την εφαρμογή ενημέρωση του υλικολογισμικού για το δείγμα συσκευή Intel Edison.

Αυτό το πρόγραμμα εκμάθησης σας δείχνει τον τρόπο για να:

- Δημιουργία μιας εφαρμογής κονσόλας που καλεί τη μέθοδο firmwareUpdate απευθείας στη συσκευή προσομοιωμένη μέσω κέντρο IoT σας.
- Δημιουργήστε μια πρόχειρη συσκευή που υλοποιεί μια άμεση μέθοδος firmwareUpdate θα μεταφερθείτε μέσω μια διαδικασία πολλών σταδίων που χρειάζεται να περιμένει για να κάνετε λήψη της εικόνας υλικολογισμικού, να κάνει λήψη της εικόνας υλικολογισμικού και τέλος ισχύει οστό υλικολογισμικού εικόνα.  Σε ολόκληρη την εκτέλεση κάθε στάδιο η συσκευή χρησιμοποιεί το διπλού αναφερθεί ιδιότητες, για να ενημερώσετε την εξέλιξη.

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε δύο Node.js κονσόλα εφαρμογών:

**dmpatterns_fwupdate_service.js**, που καλεί μια μέθοδο απευθείας στη συσκευή προσομοιωμένη, εμφανίζει την απάντηση και περιοδικά (κάθε 500ms) εμφανίζει το ενημερωμένο συσκευή διπλού αναφερθεί ιδιότητες.

**dmpatterns_fwupdate_device.js**, που συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα, λαμβάνει μια άμεση μέθοδος firmwareUpdate, εκτελείται μέσω μιας διαδικασίας πολλαπλών καταστάσεων να προσομοιώνουν μια ενημερωμένη έκδοση υλικολογισμικού συμπεριλαμβανομένων: αναμονή για τη λήψη της εικόνας, τη λήψη η νέα εικόνα και, τέλος εφαρμόζοντας την εικόνα.


Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

Έκδοση node.js 0.12.x ή νεότερη έκδοση, <br/>  [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Node.js για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

Παρακολούθηση στο άρθρο [Γρήγορα αποτελέσματα με τη Διαχείριση συσκευών](iot-hub-device-management-get-started.md) για να δημιουργήσετε το Κέντρο IoT σας και να σας συμβολοσειρά σύνδεσης.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που αποκρίνεται σε μια άμεση μέθοδο που ονομάζεται από το cloud, η οποία ενεργοποιεί μια ενημέρωση του υλικολογισμικού προσομοιωμένη συσκευής και χρησιμοποιεί τη συσκευή διπλού αναφερθεί ιδιότητες, για να ενεργοποιήσετε τα ερωτήματα διπλού συσκευή για να προσδιορίσετε συσκευές και όταν αυτά τελευταία επανεκκίνηση.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **manageddevice**.  Στο φάκελο **manageddevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών.  Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```
    
2. Στο σας εντολών στο φάκελο **manageddevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το **azure-iot-device@dtpreview** πακέτο SDK συσκευή και **azure-iot-device-mqtt@dtpreview** πακέτου:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **dmpatterns_fwupdate_device.js** στο φάκελο **manageddevice** .

4. Προσθέστε τα εξής 'απαίτηση' προτάσεις κατά την εκκίνηση του αρχείου **dmpatterns_fwupdate_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Προσθέστε μια μεταβλητή **συμβολοσειρά σύνδεσης** και χρησιμοποιήστε το για να δημιουργήσετε ένα πρόγραμμα-πελάτη συσκευή.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Προσθέστε την ακόλουθη συνάρτηση που θα χρησιμοποιηθεί για να ενημερώσετε διπλού αναφερθεί ιδιότητες

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Προσθέστε τις ακόλουθες συναρτήσεις που θα προσομοιώνουν τη λήψη και εφαρμογή της εικόνας υλικολογισμικό.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Προσθέστε την ακόλουθη συνάρτηση η οποία θα ενημερώσει την κατάσταση ενημέρωσης υλικολογισμικού μέσω του διπλού αναφέρεται ιδιότητες να περιμένουν για να κάνετε λήψη.  Συνήθως, συσκευές να ενημερώνονται ενημέρωσης διαθέσιμο και ένας διαχειριστής που ορίζονται από το αιτίες για την πολιτική τη συσκευή για να ξεκινήσετε τη λήψη και εφαρμογή της ενημερωμένης έκδοσης.  Αυτό είναι όπου θα εκτελεστεί η λογική για να ενεργοποιήσετε αυτήν την πολιτική.  Για λόγους ευκολίας, θα σας είστε να καθυστερήσει για 4 δευτερόλεπτα και να συνεχίσετε να κάνετε λήψη της εικόνας υλικολογισμικό. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Προσθέστε την ακόλουθη συνάρτηση η οποία θα ενημερώσει την κατάσταση ενημέρωσης υλικολογισμικού μέσω του διπλού αναφερθεί ιδιότητες για τη λήψη της εικόνας υλικολογισμικό.  Το ακολουθεί προς τα επάνω με προσομοίωση λήψη υλικολογισμικού και τέλος ενημερώνει την κατάσταση ενημέρωση του υλικολογισμικού για να σας ενημερώσει για λήψη επιτυχίας ή αποτυχίας.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Προσθέστε την ακόλουθη συνάρτηση η οποία θα ενημερώσει την κατάσταση ενημέρωσης υλικολογισμικού μέσω του διπλού αναφερθεί ιδιότητες για την εφαρμογή του ειδώλου υλικολογισμικό.  Το ακολουθεί προς τα επάνω με προσομοίωση μια εφαρμογή της εικόνας υλικολογισμικού και τέλος ενημερώνει την κατάσταση ενημέρωση του υλικολογισμικού για να ενημερώσετε μια εφαρμογή επιτυχίας ή αποτυχίας.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Προσθέστε το παρακάτω functoin το οποίο χειρίζεται τη μέθοδο firmwareUpdate και να ξεκινήσετε τη διαδικασία της ενημέρωσης πολλαπλών σταδίων υλικολογισμικού.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Τέλος, προσθέστε τον ακόλουθο κώδικα, το οποίο συνδέεται με διανομέα IoT ως συσκευή, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως μια εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Ενεργοποιεί μια ενημερωμένη έκδοση απομακρυσμένης υλικολογισμικού στη συσκευή τη μέθοδο του απευθείας 

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που ξεκινά μια ενημερωμένη έκδοση υλικολογισμικού απομακρυσμένο σε συσκευή με τη μέθοδο του απευθείας και χρησιμοποιεί τη συσκευή διπλού ερωτήματα για περιοδικά γρήγορα την κατάσταση της ενημερωμένης έκδοσης του ενεργού υλικολογισμικού αυτής της συσκευής.


1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **triggerfwupdateondevice**.  Στο φάκελο **triggerfwupdateondevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών.  Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```
    
2. Στο σας εντολών στο φάκελο **triggerfwupdateondevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο SDK συσκευή **azure iothub** και πακέτο **azure-iot-συσκευή-mqtt** :

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **dmpatterns_getstarted_service.js** στο φάκελο **triggerfwupdateondevice** .

4. Προσθέστε τα εξής 'απαίτηση' προτάσεις κατά την εκκίνηση του αρχείου **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Προσθέστε τις ακόλουθες δηλώσεις μεταβλητών και αντικαταστήστε τις τιμές του πλαισίου κράτησης θέσης:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Προσθέστε την ακόλουθη συνάρτηση για να βρείτε και εμφάνισης την τιμή του firmwareUpdate ανέφεραν την ιδιότητα.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Προσθέστε την ακόλουθη συνάρτηση για να καλέσετε τη μέθοδο firmwareUpdate για να κάνετε επανεκκίνηση της συσκευής προορισμού:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Τέλος, προσθέστε την ακόλουθη συνάρτηση στον κώδικα για να ξεκινήσετε το υλικολογισμικό ενημέρωση ακολουθίας και ξεκινήστε περιοδικά που εμφανίζει το διπλού αναφερθεί Ιδιότητες:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Αποθηκεύστε και κλείστε το αρχείο **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Στη γραμμή-εντολών στο φάκελο **manageddevice** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσει η ακρόαση για την άμεση μέθοδος επανεκκινήστε τον υπολογιστή.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Στη γραμμή εντολών εντολή στο φάκελο **triggerfwupdateondevice** , εκτελέστε την ακόλουθη εντολή έναυσμα η απομακρυσμένη επανεκκινήστε τον υπολογιστή και ερωτήματος για τη συσκευή διπλού για να βρείτε την τελευταία επανεκκίνηση της ώρας.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Θα δείτε το αντιδρούν τη μέθοδο απευθείας από την εκτύπωση του μηνύματος

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, που χρησιμοποιήσατε μια άμεση μέθοδος για να ενεργοποιήσετε μια ενημερωμένη έκδοση υλικολογισμικού απομακρυσμένο σε μια συσκευή και περιοδικά χρησιμοποιούνται αναφέρει το διπλού ιδιότητες για να κατανοήσετε την πρόοδο του υλικολογισμικού διαδικασία ενημερωμένων εκδόσεων.  

Για να μάθετε πώς μπορείτε να επεκτείνετε το IoT λύση και χρονοδιάγραμμα μέθοδο κλήσεις σε πολλές συσκευές, ανατρέξτε στο θέμα το [Χρονοδιάγραμμα και εκπομπής εργασιών] [ lnk-tutorial-jobs] το πρόγραμμα εκμάθησης.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
