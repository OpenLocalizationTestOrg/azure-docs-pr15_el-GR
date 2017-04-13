<properties
 pageTitle="Οδηγός για προγραμματιστές - γλώσσα ερωτημάτων | Microsoft Azure"
 description="Οδηγός Azure προγραμματιστής διανομέα IoT - περιγραφή της γλώσσας ερωτήματος που χρησιμοποιείται για την ανάκτηση πληροφορίες σχετικά με την twins, μεθόδους και εργασιών από το Κέντρο IoT σας"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Αναφορά - γλώσσα ερωτημάτων για twins και εργασιών

## <a name="overview"></a>Επισκόπηση

Ενότητα IoT παρέχει μια ισχυρή γλώσσα μοιάζει με SQL για να ανακτήσετε πληροφορίες σχετικά με [τη συσκευή twins] [ lnk-twins] και [εργασιών][lnk-jobs]. Αυτό το άρθρο παρουσιάζει:

* Μια εισαγωγή σχετικά με τις κύριες δυνατότητες της γλώσσας ερωτήματος του IoT διανομέα, και
* Λεπτομερή περιγραφή της γλώσσας.

## <a name="getting-started-with-twin-queries"></a>Γρήγορα αποτελέσματα με τα ερωτήματα διπλού

[Συσκευή twins] [ lnk-twins] μπορεί να περιέχει αντικείμενα αυθαίρετο JSON ως ετικέτες και ιδιότητες. IoT διανομέα σας επιτρέπει να twins συσκευή ερωτήματος ως ένα μεμονωμένο έγγραφο JSON που περιέχει όλες τις πληροφορίες διπλού.
Για παράδειγμα, ας υποθέσουμε ότι σας twins διανομέα IoT έχει την εξής δομή:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

Ενότητα IoT εκθέτει το twins συσκευή ως μια συλλογή εγγράφων που ονομάζεται **συσκευές**.
Επομένως, το ακόλουθο ερώτημα ανακτά ολόκληρο το σύνολο των twins συσκευή:

        SELECT * FROM devices

> [AZURE.NOTE] [SDK διανομέα IoT] [ lnk-hub-sdks] υποστηρίζει σελιδοποίηση μεγάλο αποτελεσμάτων.

IoT διανομέα σας επιτρέπει να ανακτήσετε twins φιλτράρισμα με αυθαίρετο συνθήκες. Για παράδειγμα,

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

ανακτά την twins με την ετικέτα **location.region** Ορισμός **μας**.
Οι τελεστές Boolean και αριθμητικοί συγκρίσεις υποστηρίζονται επίσης, π.χ.

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

ανακτά όλα twins βρίσκεται στις Ηνωμένες Πολιτείες έχει ρυθμιστεί για την αποστολή τηλεμετρίας λιγότερο συχνά από όσο κάθε λεπτό. Για τη διευκόλυνσή, είναι επίσης πιθανό να χρησιμοποιήσετε τις σταθερές πίνακα σε συνδυασμό με **το και τελεστές **NIN** (όχι σε)** . Για παράδειγμα,

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

ανακτά όλα twins που αναφέρεται μέσω Wi-Fi ή ενσύρματη σύνδεση. Ανατρέξτε στον [όρο WHERE] [ lnk-query-where] στην ενότητα για την πλήρη αναφορά τις δυνατότητες φιλτραρίσματος.

Ομαδοποίηση και συναθροίσεις επίσης υποστηρίζονται. Για παράδειγμα,

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Επιστρέφει το πλήθος των συσκευών σε κάθε κατάσταση ρύθμισης παραμέτρων τηλεμετρίας.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Το παραπάνω παράδειγμα απεικονίζει μια κατάσταση όπου τρεις συσκευές αναφέρει επιτυχής ρύθμισης παραμέτρων, δύο εξακολουθεί να εφαρμογή των ρυθμίσεων παραμέτρων, και ένα αναφέρει ένα σφάλμα.

### <a name="c-example"></a>Παράδειγμα C#

Η λειτουργικότητα ερωτήματος εκτίθεται από την [υπηρεσία C# SDK] [ lnk-hub-sdks] σε την τάξη **RegistryManager** .
Ακολουθεί ένα παράδειγμα ενός απλού ερωτήματος:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Παρατηρήστε πώς το αντικείμενο **ερωτήματος** έχει δημιουργηθεί με ένα μέγεθος σελίδας (έως και 1000) και, στη συνέχεια, πολλές σελίδες μπορούν να ανακτηθούν καλώντας τις μεθόδους **GetNextAsTwinAsync** πολλές φορές.
Είναι σημαντικό να λάβετε υπόψη ότι το αντικείμενο ερωτήματος εκθέτει πολλαπλάσιο **επόμενη\***, ανάλογα με την επιλογή αποσειριοποίησης που απαιτούνται από το ερώτημα, δηλαδή διπλού ή εργασία αντικειμένων ή απλό Json για χρήση κατά τη χρήση προβλέψεις.

### <a name="node-example"></a>Παράδειγμα κόμβου

Η λειτουργικότητα ερωτήματος εκτίθεται από την [υπηρεσία κόμβο SDK] [ lnk-hub-sdks] στο το το αντικείμενο **μητρώου** .
Ακολουθεί ένα παράδειγμα ενός απλού ερωτήματος:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Παρατηρήστε πώς το αντικείμενο **ερωτήματος** έχει δημιουργηθεί με μέγεθος σελίδας (έως και 1000) και, στη συνέχεια, πολλές σελίδες μπορούν να ανακτηθούν καλώντας τις μεθόδους **nextAsTwin** πολλές φορές.
Είναι σημαντικό να λάβετε υπόψη ότι το αντικείμενο ερωτήματος εκθέτει πολλαπλάσιο **επόμενη\***, ανάλογα με την επιλογή αποσειριοποίησης που απαιτούνται από το ερώτημα, δηλαδή διπλού ή εργασία αντικειμένων ή απλό Json για χρήση κατά τη χρήση προβλέψεις.

### <a name="limitations"></a>Περιορισμοί

Προς το παρόν, προβλέψεις υποστηρίζονται μόνο κατά τη χρήση συγκεντρώσεις, δηλαδή να χρησιμοποιήσετε μόνο τα μη συγκεντρωτικές ερωτήματα `SELECT *`. Επίσης, συνάθροιση υποστηρίζονται μόνο σε συνδυασμό με την ομαδοποίηση.

## <a name="getting-started-with-jobs-queries"></a>Γρήγορα αποτελέσματα με τα ερωτήματα εργασίες

[Εργασίες] [ lnk-jobs] παρέχει κάποιο τρόπο για να εκτελεί λειτουργίες στα σύνολα των συσκευών. Κάθε συσκευή διπλού περιέχει τις πληροφορίες από τις εργασίες που είναι μέρος σε μια συλλογή που ονομάζεται **εργασίες**.
Λογικά,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Προς το παρόν, αυτή η συλλογή είναι queriable ως **devices.jobs** στη γλώσσα ερωτημάτων IoT διανομέα.

Για παράδειγμα, για να δείτε όλες τις εργασίες (προηγούμενες και προγραμματισμένη) που επηρεάζουν μία μόνο συσκευή, μπορείτε να χρησιμοποιήσετε το ακόλουθο ερώτημα:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Παρατηρήστε πώς αυτό το ερώτημα παρέχει την κατάσταση ειδικά για τη συσκευή (και πιθανώς η απόκριση άμεση μέθοδος) κάθε εργασίας που επιστρέφονται.
Επίσης, είναι δυνατό να φιλτράρετε με αυθαίρετο δυαδική συνθήκες σε όλες τις ιδιότητες των αντικειμένων στη συλλογή **devices.jobs** .
Για παράδειγμα, το ακόλουθο ερώτημα:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

ανακτά όλα ολοκληρωμένη διπλού ενημέρωση εργασιών για τη συσκευή **myDeviceId** που έχουν δημιουργηθεί μετά Σεπτεμβρίου 2016.

Επίσης, είναι δυνατό να ανακτήσετε τα αποτελέσματα ανά συσκευή από μία μόνο εργασία.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Περιορισμοί
Προς το παρόν, δεν υποστηρίζουν ερωτήματα στην **devices.jobs** :

* Προβλέψεις, δηλαδή μόνο `SELECT *` είναι πιθανό;
* Συνθήκες που αναφέρονται σε τη συσκευή διπλού εκτός από τις ιδιότητες εργασίας, όπως φαίνεται παραπάνω;
* Εκτέλεση συναθροίσεις, π.χ., πλήθος, Μέσος_όρος, ομαδοποίηση κατά.

## <a name="basics-of-an-iot-hub-query"></a>Βασικά στοιχεία για ένα ερώτημα IoT διανομέα

Κάθε ερώτημα διανομέα IoT αποτελείται από μια ΕΠΙΛΟΓΉ και από όρους και προαιρετικός όρος WHERE και ΟΜΑΔΟΠΟΊΗΣΗ κατά όρους. Κάθε ερώτημα εκτελείται σε μια συλλογή εγγράφων JSON, π.χ. twins συσκευή. ΑΠΌ τον όρο FROM υποδεικνύει τη συλλογή εγγράφων για να είναι iterated (**συσκευές** ή **devices.jobs**). Στη συνέχεια, έχει εφαρμοστεί το φίλτρο στον όρο WHERE. Στην περίπτωση συναθροίσεις, τα αποτελέσματα αυτού του βήματος ομαδοποιούνται ως που καθορίζονται στον όρο GROUP BY και, για κάθε ομάδα, δημιουργείται μια γραμμή όπως καθορίζεται στον όρο SELECT.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>ΑΠΌ τον όρο FROM

Ο όρος **από < from_specification >** μπορεί να αναλάβει μόνο δύο τιμές: **από συσκευές**, ερωτήματος twins συσκευή ή **από devices.jobs**, για λεπτομέρειες ανά συσκευή εργασίας ερωτήματος.

## <a name="where-clause"></a>Πρόταση WHERE

Το **όπου < filter_condition >** όρος είναι προαιρετικό. Καθορίζει το συνθηκών που πρέπει να ικανοποιεί τα έγγραφα JSON στη συλλογή από για να περιλαμβάνονται ως τμήμα του αποτελέσματος. Οποιοδήποτε έγγραφο JSON πρέπει να αξιολογήσετε τις καθορισμένες συνθήκες σε "true" που θα συμπεριληφθούν στο αποτέλεσμα.

Οι επιτρεπόμενοι συνθήκες περιγράφονται στην ενότητα [παραστάσεις και τις προϋποθέσεις][lnk-query-expressions].

## <a name="select-clause"></a>Όρος SELECT

Ο όρος SELECT (**ΕΠΙΛΈΞΤΕ < select_list >**) είναι υποχρεωτικό και καθορίζει ποιες τιμές θα να ανακτηθούν από το ερώτημα. Καθορίζει τις τιμές JSON που θα χρησιμοποιηθεί για τη δημιουργία νέων αντικειμένων JSON για κάθε στοιχείο του υποσυνόλου φιλτραρισμένη (και προαιρετικά ομαδοποιημένες) από τη συλλογή από, η φάση προβολής δημιουργεί ένα νέο αντικείμενο JSON, συνταχθεί με τις τιμές που καθορίζονται στον όρο SELECT.

Αυτή είναι η γραμματική ο όρος SELECT:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

όπου **attribute_name** αναφέρεται σε οποιαδήποτε ιδιότητα του εγγράφου JSON στη συλλογή από. Μπορείτε να βρείτε ορισμένα παραδείγματα όροι SELECT τα [Γρήγορα αποτελέσματα με τα ερωτήματα διπλού] [ lnk-query-getstarted] ενότητας.

Προς το παρόν, επιλογή όρους διαφορετικά από **ΕΠΙΛΈΞΤΕ \* ** υποστηρίζονται μόνο σε σύνθετα ερωτήματα στην twins.

## <a name="group-by-clause"></a>Όρο GROUP BY

Ο όρος **GROUP BY < group_specification >** είναι ένα προαιρετικό βήμα που μπορεί να εκτελεστεί μετά το φίλτρο που καθορίζεται στον όρο WHERE και πριν από την προβολή που καθορίζεται στο πλαίσιο ΕΠΙΛΟΓΉ. Ομαδοποιεί τα έγγραφα που βασίζονται σε την τιμή του χαρακτηριστικού. Αυτές οι ομάδες που χρησιμοποιούνται για τη δημιουργία συγκεντρωτικές τιμές όπως καθορίζεται στον όρο SELECT.

Ένα παράδειγμα ενός ερωτήματος με χρήση GROUP BY είναι:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Είναι η επίσημη σύνταξη για ΟΜΑΔΟΠΟΊΗΣΗ κατά:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

όπου **attribute_name** αναφέρεται σε οποιαδήποτε ιδιότητα του εγγράφου JSON στη συλλογή από. 

Προς το παρόν, ο όρος GROUP BY υποστηρίζεται μόνο κατά την υποβολή ερωτήματος twins.

## <a name="expressions-and-conditions"></a>Παραστάσεις και τις προϋποθέσεις

Σε υψηλό επίπεδο, μια *παράσταση*:

* Αξιολογεί μια παρουσία τύπου JSON (δηλαδή Boolean, αριθμό, συμβολοσειρά, πίνακα ή αντικείμενο), και
* Έχει οριστεί περιστρέφοντας δεδομένα που προέρχονται από το έγγραφο JSON συσκευή και σταθερές με χρήση ενσωματωμένων τελεστές και συναρτήσεις.

*Οι συνθήκες* είναι παραστάσεις που αξιολογείται σε μια τιμή Boolean, δηλαδή οποιαδήποτε σταθερά διαφορετικά από δυαδική **τιμή true** θεωρείται ως **false** (με **τιμή null**, **δεν έχει οριστεί**, την παρουσία οποιοδήποτε αντικείμενο ή έναν πίνακα, οποιαδήποτε συμβολοσειρά και με σαφήνεια τη δυαδική **τιμή false**).

Η σύνταξη για παραστάσεις είναι:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

όπου:

| Σύμβολο | Ορισμός |
| -------------- | -----------------|
| attribute_name | Οποιαδήποτε ιδιότητα του εγγράφου JSON στη συλλογή από. |
| unary_operator | Οποιαδήποτε μοναδιαίος τελεστής σύμφωνα με την ενότητα τελεστές. |
| binary_operator | Οποιαδήποτε δυαδικό τελεστή σύμφωνα με την ενότητα τελεστές. |
| decimal_literal | Μια Αιώρηση εκφρασμένη σε δεκαδική μορφή. |
| hexadecimal_literal | Ένας αριθμός που εκφράζεται από τη συμβολοσειρά '0 x' ακολουθούμενο από μια συμβολοσειρά δεκαεξαδικό αριθμό ψηφίων. |
| string_literal | Συμβολοσειρές κειμένου είναι συμβολοσειρές Unicode που αντιπροσωπεύονται από μια ακολουθία μηδέν ή σε περισσότερες χαρακτήρες Unicode ή ακολουθίες διαφυγής. Συμβολοσειρές κειμένου περικλείεται σε μονά εισαγωγικά (απόστροφος: ') ή διπλά εισαγωγικά (εισαγωγικό: "). Επιτρέπεται μεταβαίνει απευθείας: `\'`, `\"`, `\\`, `\uXXXX` για χαρακτήρες Unicode που ορίζονται από 4 δεκαεξαδικό αριθμό ψηφίων. |

### <a name="operators"></a>Τελεστές

Υποστηρίζονται τους ακόλουθους τελεστές:

| Οικογένεια | Τελεστές |
| -------------- | -----------------|
| Αριθμητικός μέσος | +,-,*,/,% |
| Λογική | ΚΑΙ, Ή ΌΧΙ |
| Σύγκριση |  =,! =, <>,, < =, > =, <> |

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε πώς μπορείτε να εκτελείτε ερωτήματα με τις εφαρμογές σας χρησιμοποιώντας [SDK διανομέα IoT][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md