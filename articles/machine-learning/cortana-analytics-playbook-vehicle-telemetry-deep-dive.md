<properties 
    pageTitle="Playbook λύση ανάλυση τηλεμετρίας οχήματος: βαθύ περισσότερα για τη λύση | Microsoft Azure" 
    description="Χρησιμοποιήστε τις δυνατότητες της Cortana πληροφοριών για να αποκτήσετε σε πραγματικό χρόνο και πρόβλεψης ιδεών οχήματος εύρυθμης λειτουργίας και την οι συνήθειές όσον αφορά." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Playbook λύση ανάλυση τηλεμετρίας οχήματος: βαθύ κατάρρευση σε της λύσης

Αυτό **μενού** συνδέσεις στις ενότητες του playbook αυτό: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Αυτή η ενότητα drills προς τα κάτω σε κάθε ένα από τα στάδια που παρουσιάζεται η αρχιτεκτονική λύσης με οδηγίες και υποδείξεις για την προσαρμογή. 

## <a name="data-sources"></a>Προελεύσεις δεδομένων

Η λύση χρησιμοποιεί δύο διαφορετικές προελεύσεις δεδομένων:

- **προσομοιωμένη οχήματος σήματα και διαγνωστικών συνόλου δεδομένων** και 
- **Κατάλογος οχήματος**

Προσομοιωτή τηλεματικές οχήματος περιλαμβάνεται ως τμήμα αυτής της λύσης. Το εκπέμπει διαγνωστικών πληροφοριών και σήματα αντιστοιχεί στην κατάσταση του οχήματος και για το μοτίβο οδήγησης σε μια δεδομένη χρονική στιγμή. Κάντε κλικ στην επιλογή [Simulator τηλεματικές οχήματος](http://go.microsoft.com/fwlink/?LinkId=717075) για να κάνετε λήψη της **Οχήματος τηλεματικές Simulator Visual Studio λύσης** για τις προσαρμογές σύμφωνα με τις απαιτήσεις σας. Ο κατάλογος οχήματος περιέχει ένα σύνολο δεδομένων αναφοράς με μια Κατάσταση αντιστοίχιση μοντέλο.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Εικόνα 2-Simulator τηλεματικές οχήματος*

Αυτό είναι ένα σύνολο JSON μορφοποίηση δεδομένων που περιέχει την ακόλουθη διάταξη.

Στήλη | Περιγραφή | Τιμές 
 ------- | ----------- | --------- 
ΚΑΤΆΣΤΑΣΗ | Αναγνωριστικός αριθμός που δημιούργησε τυχαία οχήματος | Αυτό είναι που λαμβάνονται από μια κύρια λίστα 10.000 που δημιούργησε τυχαία οχήματος αναγνωριστικούς αριθμούς.
Εξωτερική θερμοκρασία | Την εξωτερική θερμοκρασία όπου καθορίζει οχήματος | Τυχαίος αριθμός από 0-100
Μηχανισμός θερμοκρασίας | Η θερμοκρασία μηχανισμός του οχήματος | Τυχαίος αριθμός από 0-500
Ταχύτητα | Η ταχύτητα μηχανισμός την οποία καθορίζει οχήματος | Τυχαίος αριθμός από 0-100
Καυσίμου | Το επίπεδο καυσίμου του οχήματος | Τυχαίος αριθμός από 0-100 (υποδεικνύει το ποσοστό του επιπέδου καυσίμου)
EngineOil | Το επίπεδο Γεωργίου μηχανισμός του οχήματος | Τυχαίος αριθμός από 0-100 (υποδεικνύει το ποσοστό του επιπέδου Γεωργίου μηχανισμός)
Πίεση ελαστικού | Η πίεση ελαστικού του οχήματος | Τυχαίος αριθμός από 0-50 (υποδεικνύει το ποσοστό του επιπέδου πίεση ελαστικού)
Οδομετρητή | Η ένδειξη οδομετρητή του οχήματος | Τυχαίος αριθμός από 0-200000
Accelerator_pedal_position | Η θέση ποδόπληκτρο επιτάχυνσης του οχήματος | Τυχαίος αριθμός από 0-100 (υποδεικνύει το ποσοστό του επιπέδου επιτάχυνσης)
Parking_brake_status | Υποδεικνύει εάν οχήματος είναι σε στάθμευση ή όχι | TRUE ή False
Headlamp_status | Υποδεικνύει πού είναι προβολέα στην ή όχι | TRUE ή False
Brake_pedal_status | Υποδεικνύει αν το ποδόπληκτρο πέδησης είναι πατημένο ή όχι | TRUE ή False
Transmission_gear_position | Η θέση γραναζιού μετάδοση του οχήματος | Μέλη: πρώτα, το δεύτερο, τρίτο, τέταρτη, Πέμπτη, έκτη, έβδομη, έως
Ignition_status | Υποδεικνύει εάν οχήματος είναι εκτελείται ή έχει διακοπεί | TRUE ή False
Windshield_wiper_status | Υποδεικνύει εάν η υαλοκαθαριστήρα ανεμοθώρακα είναι ενεργοποιημένη ή όχι | TRUE ή False
ABS | Υποδεικνύει εάν έχει αναλάβει ABS ή όχι | TRUE ή False
Χρονική σήμανση | Η σήμανση χρόνου όταν δημιουργείται το σημείο δεδομένων | Ημερομηνία
Πόλη | Η θέση του οχήματος | 4 πόλεις σε αυτήν τη λύση: Μπέλβιου, Redmond, Sammamish, Σιάτλ


Το σύνολο δεδομένων αναφοράς μοντέλο οχήματος περιέχει Κατάσταση στην αντιστοίχιση μοντέλο. 

ΚΑΤΆΣΤΑΣΗ | Μοντέλο |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedan |
8J0U8XCPRGW4Z3NQE | Υβριδική |
WORG68Z2PLTNZDBI7 | Οικογενειακό χάρη λιμουζίνα |
JTHMYHQTEPP4WBMRN | Sedan |
W9FTHG27LZN1YWO0Y | Υβριδική |
MHTP9N792PHK08WJM | Οικογενειακό χάρη λιμουζίνα |
EI4QXI2AXVQQING4I | Sedan |
5KKR2VB4WHQH97PF8 | Υβριδική |
W9NSZ423XZHAONYXB | Οικογενειακό χάρη λιμουζίνα |
26WJSGHX4MA5ROHNL | Δυνατότητα μετατροπής |
GHLUB6ONKMOSI7E77 | Θέση οχήματος |
9C2RHVRVLMEJDBXLP | Συμπαγής αυτοκινήτου |
BRNHVMZOUJ6EOCP32 | Μικρό SUV |
VCYVW0WUZNBTM594J | Σπορ αυτοκίνητο |
HNVCE6YFZSA5M82NY | Ενδιάμεση SUV |
4R30FOR7NUOBL05GJ | Θέση οχήματος |
WYNIIY42VKV6OQS1J | Μεγάλη SUV |
8Y5QKG27QET1RBK7I | Μεγάλη SUV |
DF6OX2WSRA6511BVG | Κουπέ |
Z2EOZWZBXAEW3E60T | Sedan |
M4TV6IEALD5QDS3IR | Υβριδική |
VHRA1Y2TGTA84F00H | Οικογενειακό χάρη λιμουζίνα |
R0JAUHT1L1R3BIKI0 | Sedan |
9230C202Z60XX84AU | Υβριδική |
T8DNDN5UDCWL7M72H | Οικογενειακό χάρη λιμουζίνα |
4WPYRUZII5YV7YA42 | Sedan |
D1ZVY26UV2BFGHZNO | Υβριδική |
XUF99EW9OIQOMV7Q7 | Οικογενειακό χάρη λιμουζίνα
8OMCL3LGI7XNCC21U | Δυνατότητα μετατροπής |
…….  |   |


### <a name="to-generate-simulated-data"></a>Για τη δημιουργία προσομοιωμένη δεδομένων
1.  Για να κάνετε λήψη του πακέτου simulator δεδομένων, κάντε κλικ στο βέλος στην επάνω δεξιά γωνία του κόμβου οχήματος τηλεματικές Simulator. Αποθηκεύστε και εξαγάγετε τα αρχεία τοπικά στον υπολογιστή σας. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Εικόνα 3-οχήματος Τηλεμετρίας ανάλυση λύση σχέδιο*

2.  Στον τοπικό υπολογιστή σας, μεταβείτε στο φάκελο όπου εξαγάγατε το πακέτο οχήματος τηλεματικές Simulator. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Εικόνα 4 – φάκελο Simulator τηλεματικές οχήματος*

3.  Εκτελέστε την εφαρμογή **CarEventGenerator.exe**.

### <a name="references"></a>Αναφορές

[Λύση Visual Studio Simulator τηλεματικές οχήματος](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Ο διανομέας Azure συμβάντος](https://azure.microsoft.com/services/event-hubs/)

[Εργοστασιακές Azure δεδομένων](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Κατάποσης
Συνδυασμοί Azure συμβάν διανομείς, ροή ανάλυση και εργοστασίου δεδομένων είναι δυνατή για να ingest τα σήματα οχήματος, τα συμβάντα διαγνωστικών, και σε πραγματικό χρόνο και μαζικών ανάλυσης. Όλα αυτά τα στοιχεία δημιουργούνται και έχει ρυθμιστεί ως μέρος του την ανάπτυξη της λύσης. 

### <a name="real-time-analysis"></a>Ανάλυση σε πραγματικό χρόνο
Τα συμβάντα που δημιουργούνται από το Simulator τηλεματικές οχήματος δημοσιεύονται στο διανομέα συμβάν χρησιμοποιώντας το SDK διανομέα συμβάν. Η εργασία ροής ανάλυση ingests αυτά τα συμβάντα από την ενότητα συμβάντων και επεξεργάζεται τα δεδομένα σε πραγματικό χρόνο για την ανάλυση της εύρυθμης λειτουργίας οχήματος. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Εικόνα 5 - διανομέα συμβάν πίνακα εργαλείων*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Εικόνα 6 - ροή εργασίας ανάλυση επεξεργασία δεδομένων*

Η εργασία ανάλυσης ροή;

- ingests δεδομένων από την ενότητα συμβάντος 
- εκτελεί ένας σύνδεσμος με τα δεδομένα αναφοράς για να αντιστοιχίσετε την Κατάσταση οχήματος με το αντίστοιχο υπόδειγμα 
- εξακολουθεί να εμφανίζεται τους στο χώρο αποθήκευσης αντικειμένων blob του Azure για ανάλυση εμπλουτισμένου δέσμης. 

Το παρακάτω ερώτημα ανάλυση ροή χρησιμοποιείται για να διατηρούνται τα δεδομένα στο χώρο αποθήκευσης αντικειμένων blob του Azure. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Εικόνα 7 - ροή αναλυτικών στοιχείων εργασία ερωτήματος για δεδομένα κατάποσης*

### <a name="batch-analysis"></a>Μαζική ανάλυση
Θα σας επίσης δημιουργούν μια επιπλέον όγκου σήματα προσομοιωμένη οχήματος και διαγνωστικών σύνολο δεδομένων για πιο πλούσια δέσμη ανάλυση. Αυτό είναι απαραίτητο για να βεβαιωθείτε ότι ενός τόμου καλή αντιπρόσωπος δεδομένων για μαζική επεξεργασία. Για το σκοπό αυτό χρησιμοποιούμε μια διαδικασία με το όνομα "PrepareSampleDataPipeline" στη ροή εργασίας Azure εργοστασίου δεδομένων για τη δημιουργία ενός έτους αξία σήματα προσομοιωμένη οχήματος και διαγνωστικών συνόλου δεδομένων. Κάντε κλικ στην επιλογή [προσαρμοσμένη δραστηριότητα εργοστασίου δεδομένων](http://go.microsoft.com/fwlink/?LinkId=717077) για να κάνετε λήψη του εργοστασίου δεδομένων προσαρμοσμένη DotNet δραστηριότητα λύση Visual Studio για τις προσαρμογές σύμφωνα με τις απαιτήσεις σας. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Εικόνα 8 - Προετοιμασία δειγμάτων δεδομένων για τη ροή εργασίας μαζική επεξεργασία*

Η διαδικασία αποτελείται από ένα προσαρμοσμένο .net ADF δραστηριότητα, εμφάνιση εδώ:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Σχήμα 9 - PrepareSampleDataPipeline*

Όταν εκτελεί τη διαδικασία με επιτυχία και έχει επισημανθεί "Είστε έτοιμοι", "RawCarEventsTable" σύνολο δεδομένων ενός έτους αξίζει σημάτων προσομοιωμένη οχήματος και διαγνωστικών παράγονται δεδομένων. Δείτε το παρακάτω φακέλων και αρχείων που έχουν δημιουργηθεί με το λογαριασμό χώρου αποθήκευσης κάτω από το κοντέινερ "connectedcar":

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Εικόνα 10 - PrepareSampleDataPipeline εξόδου*

### <a name="references"></a>Αναφορές

[Azure SDK διανομέα συμβάντων για κατάποση ροής](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure δυνατότητες κίνηση δεδομένων εργοστασίου δεδομένων](../data-factory/data-factory-data-movement-activities.md)
[Δραστηριότητα DotNet εργοστασίου δεδομένων Azure](../data-factory/data-factory-use-custom-activities.md)

[Λύση visual studio δραστηριότητας Azure DotNet εργοστασίου δεδομένων για την προετοιμασία δειγμάτων δεδομένων](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Διαμερίσματα του συνόλου δεδομένων

Τα ανεπεξέργαστα ημι-δομημένα οχήματος σήματα και διαγνωστικών συνόλου δεδομένων δημιουργούνται διαμερίσματα στο βήμα προετοιμασίας δεδομένων σε μια μορφή ΈΤΟΥΣ/ΜΉΝΑ. Αυτό διαμερισμάτων Προβιβάστε πιο αποτελεσματική υποβολή ερωτημάτων και μεταβλητού μεγέθους μακροπρόθεσμες χώρου αποθήκευσης, ενεργοποιώντας σφαλμάτων επάνω από το λογαριασμό μία blob στην επόμενη όταν γεμίσει ο πρώτος λογαριασμός. 

>[AZURE.NOTE] Αυτό το βήμα στη λύση εφαρμόζεται μόνο για μαζική επεξεργασία.

Εισόδου και εξόδου διαχείριση δεδομένων δεδομένων:

- Τα **δεδομένα εξόδου** (επισημαίνεται *PartitionedCarEventsTable*) είναι να διατηρηθούν για μεγάλο χρονικό διάστημα ως βασικές / "rawest" φόρμας δεδομένων του το του πελάτη "λίμνης δεδομένων". 
- Τα **δεδομένα εισόδου** για αυτήν τη διαδικασία κάνατε συνήθως απορριφθούν, καθώς τα δεδομένα εξόδου έχει πλήρη πιστότητα στην είσοδο - αποθηκεύεται μόνο (διαμερίσματα) καλύτερα για μετέπειτα χρήση.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Σχήμα 11 – Partition αυτοκίνητο συμβάντα ροής εργασίας*

Τα ανεπεξέργαστα δεδομένα είναι διαμερίσματα με μια δραστηριότητα Hive HDInsight στο "PartitionCarEventsPipeline". Το δείγμα δεδομένων που δημιουργούνται στο βήμα 1 για ένα έτος έχει διαμερίσματα ανά ΜΉΝΑ/ΈΤΟΣ. Τα διαμερίσματα που χρησιμοποιούνται για τη δημιουργία σημάτων οχήματος και διαγνωστικών δεδομένων για κάθε μήνα (συνολικό 12 διαμερίσματα) ενός έτους. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Σχήμα 12 - PartitionCarEventsPipeline*

Η ακόλουθη δέσμη ενεργειών της ομάδας, με το όνομα "partitioncarevents.hql", χρησιμοποιείται για τη δημιουργία διαμερισμάτων και βρίσκεται στο φάκελο "\demo\src\connectedcar\scripts" της λήψης zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Σχήμα 13 - PartitionConnectedCarEvents Hive δέσμης ενεργειών*

Όταν η διαδικασία εκτελείται με επιτυχία, μπορείτε να δείτε τα ακόλουθα διαμερίσματα που δημιουργούνται στο λογαριασμό σας στο χώρο αποθήκευσης κάτω από το κοντέινερ "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Σχήμα 14 - διαμερίσματα εξόδου*

Τα δεδομένα είναι βελτιστοποιημένο τώρα, είναι πιο διαχειρίσιμα και είστε έτοιμοι για περαιτέρω επεξεργασία για να αποκτήσετε εμπλουτισμένου δέσμη ιδέες. 

## <a name="data-analysis"></a>Ανάλυση δεδομένων

Σε αυτήν την ενότητα, μπορείτε να δείτε πώς μπορείτε να συνδυάσετε Azure ροή ανάλυση, Azure μηχανικής εκμάθησης, εργοστασίου δεδομένων Azure και Azure HDInsight για εμπλουτισμένο ανάλυσης για προχωρημένους σε οχήματος εύρυθμης λειτουργίας και οι συνήθειές όσον αφορά την. Υπάρχουν τρεις υποενότητες εδώ:

1.  **Μηχανικής εκμάθησης**: αυτό υπο-ενότητα περιέχει πληροφορίες σχετικά με την έρευνα εντοπισμού ανωμαλία που χρησιμοποιούνται σε αυτήν τη λύση πρόβλεψη οχήματα που απαιτεί συντήρησης συντήρησης και τους που απαιτεί υπενθυμίζει λόγω ζητημάτων ασφάλεια.
2.  **Ανάλυση σε πραγματικό χρόνο**: Αυτή η υπο-ενότητα περιέχει πληροφορίες σχετικά με την ανάλυση σε πραγματικό χρόνο με χρήση της γλώσσας ερωτήματος ανάλυσης ροής και operationalizing την έρευνα μηχανικής εκμάθησης σε πραγματικό χρόνο χρησιμοποιώντας μια προσαρμοσμένη εφαρμογή.
3.  **Μαζική ανάλυση**: Αυτή η υπο-ενότητα περιέχει πληροφορίες σχετικά με το μετασχηματισμό και την επεξεργασία των δέσμη δεδομένων με χρήση του Azure HDInsight και Azure μηχανικής εκμάθησης operationalized από προέλευση δεδομένων του Azure.

### <a name="machine-learning"></a>Μηχανικής εκμάθησης

Δείτε τα στόχος μας είναι να πρόβλεψης τα οχήματα που απαιτούν συντήρησης ή ανάκληση που βασίζεται σε συγκεκριμένα χερσότοποι στατιστικά στοιχεία. Κάνουμε οι ακόλουθες υποθέσεις

- Εάν μία από τις παρακάτω τρεις συνθήκες είναι αληθείς, τα οχήματα χρειάζεται **Συντήρηση συντήρησης**:
    - Πίεση ελαστικού είναι χαμηλή
    - Μηχανισμός Γεωργίου επίπεδο είναι χαμηλή
    - Μηχανισμός θερμοκρασίας είναι υψηλή

- Εάν μία από τις ακόλουθες συνθήκες είναι αληθείς, τα οχήματα μπορεί να υπάρχει κάποιο **ζήτημα ασφάλειας** και να απαιτείται **ανάκλησης**:
    - Μηχανισμός θερμοκρασίας είναι υψηλή, αλλά δεν περιλαμβάνονται θερμοκρασίας είναι χαμηλή
    - Μηχανισμός θερμοκρασίας είναι χαμηλή, αλλά δεν περιλαμβάνονται θερμοκρασίας είναι υψηλή

Με βάση τις προηγούμενες απαιτήσεις, έχουμε δημιουργήσει δύο ξεχωριστές μοντέλων για τον εντοπισμό ανωμαλίες, μία για τον εντοπισμό συντήρησης οχήματος και μία για τον εντοπισμό ανάκλησης οχήματος. Σε αυτές τις δύο μοντέλα, τον ενσωματωμένο αλγόριθμο κεφαλαίου στοιχείο ανάλυσης (ΣΕΣΣ) χρησιμοποιείται για τον εντοπισμό ανωμαλία. 

**Μοντέλο εντοπισμού συντήρηση**

Εάν μία από τρεις ενδείξεις ελαστικού πίεση, Γεωργίου μηχανισμός ή θερμοκρασίας μηχανισμός - δεν ικανοποιεί τη συνθήκη αντίστοιχα, του μοντέλου εντοπισμού συντήρησης αναφορές μια ανωμαλία. Ως αποτέλεσμα, μόνο πρέπει να λάβετε υπόψη αυτές τις τρεις μεταβλητές κατά τη δημιουργία του μοντέλου. Στο μας έρευνας στο Azure μηχανικής εκμάθησης, χρησιμοποιούμε πρώτα μια λειτουργική μονάδα **Επιλογή στηλών στο σύνολο δεδομένων** για να εξαγάγετε αυτές τις τρεις μεταβλητές. Επόμενο χρησιμοποιούμε τη λειτουργική μονάδα εντοπισμού βάσει ΣΕΣΣ ανωμαλία για να δημιουργήσετε το μοντέλο εντοπισμού ανωμαλία. 

Ανάλυση στοιχείο κεφάλαιο (ΣΕΣΣ) είναι μια τεχνική υπάρχοντα στο μηχανικής εκμάθησης που μπορούν να εφαρμοστούν σε δυνατότητα επιλογής, ταξινόμηση και ο εντοπισμός ανωμαλία. ΣΕΣΣ μετατρέπει ένα σύνολο υπόθεσης που περιέχει πιθανώς συσχετισμένης μεταβλητές, σε ένα σύνολο τιμών που ονομάζεται κύρια στοιχεία. Η ιδέα κλειδιού του βάσει ΣΕΣΣ μοντελοποίηση είναι στα δεδομένα του έργου σε ένα κενό διάστημα κάτω διαστάσεων ώστε δυνατότητες και ανωμαλίες να αναγνωρίζονται πιο εύκολα.
 
Για κάθε νέα εισαγωγή δεδομένων στο μοντέλο εντοπισμού, το πρόγραμμα ανίχνευσης ανωμαλία υπολογίζει πρώτα την προβολή σε το eigenvectors και, στη συνέχεια, τύπος υπολογίζει το σφάλμα κανονικοποιημένη ανακατασκευή. Αυτό το σφάλμα κανονικοποιημένη είναι στη βαθμολογία ανωμαλία. Όσο υψηλότερη το σφάλμα, η πιο ύπαρξη η παρουσία είναι. 

Με το ζήτημα εντοπισμού συντήρησης, κάθε εγγραφή μπορεί να θεωρηθεί όπως ένα σημείο σε ένα κενό διάστημα τρισδιάστατων ορίζεται από την πίεση ελαστικού Γεωργίου μηχανισμός και θερμοκρασίας μηχανισμός συντεταγμένες. Για να καταγράψετε αυτές τις ανωμαλίες, θα σας μπορεί να προβληθεί τα αρχικά δεδομένα στο τρισδιάστατων χώρο σε μια περιοχή 2 διαστάσεων με χρήση ΣΕΣΣ. Έτσι, ορίστε την παράμετρο στον αριθμό των στοιχείων για να χρησιμοποιήσετε στο ΣΕΣΣ είναι 2. Αυτή η παράμετρος ακούγεται σημαντικό ρόλο βάσει ΣΕΣΣ ανωμαλία ανίχνευσης για την εφαρμογή. Μετά την προβολής δεδομένων με χρήση ΣΕΣΣ, μπορεί να περιγράφουμε αυτές τις ανωμαλίες πιο εύκολα.

**Ανάκληση ανωμαλία Εντοπισμός μοντέλου** Στο μοντέλο εντοπισμού ανωμαλία ανάκλησης, μπορούμε να χρησιμοποιήσουμε την επιλογή στηλών στο σύνολο δεδομένων και βάσει ΣΕΣΣ ανωμαλία λειτουργικών μονάδων εντοπισμού με παρόμοιο τρόπο. Συγκεκριμένα, θα σας πρώτα να εξαγάγετε τρεις μεταβλητές - θερμοκρασίας μηχανισμός, εξωτερική θερμοκρασία και την ταχύτητα - χρήση της λειτουργικής μονάδας **Επιλογή στηλών στο σύνολο δεδομένων** . Περιλαμβάνονται επίσης τη μεταβλητή ταχύτητα εφόσον η θερμοκρασία μηχανισμός συνήθως είναι συσχετισμένη με την ταχύτητα. Επόμενο χρησιμοποιούμε λειτουργική μονάδα εντοπισμού βάσει ΣΕΣΣ ανωμαλία για το project τα δεδομένα από το χώρο τρισδιάστατων σε μια περιοχή 2 διαστάσεων. Πληρούνται τα κριτήρια ανάκλησης και οχήματος απαιτεί ανάκλησης όταν μηχανισμός θερμοκρασία και έξω θερμοκρασία ιδιαίτερα αρνητικά συσχετίζονται. Χρήση βάσει ΣΕΣΣ ανωμαλία αλγόριθμος ανίχνευσης, θα σας να καταγράψετε το ανωμαλίες μετά την εκτέλεση ΣΕΣΣ. 

Όταν εκπαίδευση υπόδειγμα, πρέπει να χρησιμοποιήσετε κανονική δεδομένα, τα οποία δεν απαιτεί συντήρησης ή ανάκληση ως δεδομένα εισόδου για την εκπαίδευση στο μοντέλο βάσει ΣΕΣΣ ανωμαλία εντοπισμού. Στο το βαθμολογίας έρευνας, μπορούμε να χρησιμοποιήσουμε το μοντέλο εντοπισμού εκπαιδευμένο ανωμαλία για τον εντοπισμό οχήματος απαιτεί συντήρησης ή ανάκληση ή όχι. 


### <a name="real-time-analysis"></a>Ανάλυση σε πραγματικό χρόνο

Το παρακάτω ερώτημα SQL ανάλυση ροή χρησιμοποιείται για να λάβετε τον μέσο όρο των όλες τις παραμέτρους σημαντικές οχήματος όπως ταχύτητα οχήματος επίπεδο καυσίμου, μηχανισμός θερμοκρασίας, οδομετρητή ανάγνωσης, πίεση ελαστικού, μηχανισμός Γεωργίου επίπεδο και άλλα άτομα. Τους μέσους όρους που χρησιμοποιούνται για να εντοπίσει ανωμαλίες, θεμάτων ειδοποιήσεις, και καθορίζουν τις συνολικές συνθήκες εύρυθμης λειτουργίας οχήματα που λειτουργούν σε συγκεκριμένη περιοχή και, στη συνέχεια, να το συσχετισμός σε κατηγορίες χρηστών που επηρεάζονται. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Σχήμα 15-ροή ανάλυση ερωτήματος για επεξεργασία σε πραγματικό χρόνο

Όλους τους μέσους όρους υπολογίζονται πάνω από ένα TumblingWindow 3 δευτερόλεπτα. Χρησιμοποιούμε TubmlingWindow σε αυτήν την περίπτωση εφόσον απαιτείται επικαλύπτονται και συνεχόμενη χρονικά διαστήματα. 

Για να μάθετε περισσότερα σχετικά με όλες τις δυνατότητες "Άνοιγμα παραθύρου" στο Azure ροή ανάλυση, κάντε κλικ στην επιλογή [Άνοιγμα παραθύρου (ανάλυση ροή Azure)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Πρόβλεψη σε πραγματικό χρόνο**

Μια εφαρμογή περιλαμβάνεται ως τμήμα της λύσης για να operationalize το μοντέλο μηχανικής εκμάθησης σε πραγματικό χρόνο. Αυτή η εφαρμογή ονομάζεται "RealTimeDashboardApp" δημιουργείται και έχει ρυθμιστεί ως μέρος του την ανάπτυξη της λύσης. Η εφαρμογή πραγματοποιεί τα εξής:

1.  Ακρόαση σε μια περίοδο λειτουργίας διανομέα συμβάν όπου ροή ανάλυσης δημοσίευσης τα συμβάντα σε ένα μοτίβο συνεχώς. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Σχήμα 16 – ροή ανάλυση ερωτήματος για τη δημοσίευση των δεδομένων για το αποτέλεσμα παρουσία διανομέα συμβάντος* 

2.  Για κάθε συμβάν που λαμβάνει αυτής της εφαρμογής: 

    - Επεξεργάζεται τα δεδομένα χρησιμοποιώντας το τελικό σημείο μηχανικής εκμάθησης απόκριση αίτησης βαθμολογίας (στο). Το τελικό σημείο στο δημοσιεύεται αυτόματα ως μέρος της ανάπτυξης.
    - Το αποτέλεσμα στο δημοσιεύεται σε ένα σύνολο δεδομένων PowerBI χρησιμοποιώντας το πάτημα APIs.

Αυτό το μοτίβο ισχύει επίσης για σενάρια στην οποία θέλετε να ενσωματώσετε μια εφαρμογή γραμμής επιχειρήσεις (LoB) με τη ροή σε πραγματικό χρόνο ανάλυση, για σενάρια όπως ειδοποιήσεις, ειδοποιήσεις και την ανταλλαγή μηνυμάτων.

Κάντε κλικ στην επιλογή [λήψη RealtimeDashboardApp](http://go.microsoft.com/fwlink/?LinkId=717078) για να κάνετε λήψη της λύσης RealtimeDashboardApp Visual Studio για τις προσαρμογές. 

**Για να εκτελέσετε την εφαρμογή πίνακα εργαλείων σε πραγματικό χρόνο**

1.  Κάντε κλικ στον κόμβο PowerBI στην προβολή διαγράμματος και κάντε κλικ στη σύνδεση "Λήψη εφαρμογής πίνακα εργαλείων σε πραγματικό χρόνο" στο παράθυρο "Ιδιότητες". ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Σχήμα 17 – PowerBI οδηγίες ρύθμισης πίνακα εργαλείων*
2.  Εξαγωγή και να αποθηκεύσετε τοπικά ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *σχήμα 18 – RealtimeDashboardApp φακέλου*
3.  Εκτέλεση της εφαρμογής RealtimeDashboardApp.exe
4.  Παροχή έγκυρα διαπιστευτήρια Power BI, πραγματοποιήστε είσοδο και κάντε κλικ στην επιλογή Αποδοχή ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Σχήμα 19 – RealtimeDashboardApp: Είσοδος στο PowerBI*

>[AZURE.NOTE] Εάν θέλετε να κάνετε εκκαθάριση του συνόλου δεδομένων PowerBI, εκτελέστε το RealtimeDashboardApp με την παράμετρο "flushdata": 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Μαζική ανάλυση

Είναι ο στόχος εδώ για να εμφανίσετε τον τρόπο μηχανές Contoso χρησιμοποιεί τις δυνατότητες Azure υπολογισμού για να εκμεταλλευτείτε μεγάλο δεδομένων για να αποκτήσετε εμπλουτισμένου ιδέες σε οδήγησης μοτίβο, συμπεριφορά χρήσης και εύρυθμης λειτουργίας οχήματος. Αυτό είναι δυνατόν να:

- Βελτίωση της εμπειρίας πελατών και να κάνετε κοστίζει με την παροχή ιδέες για την οι συνήθειές όσον αφορά και συμπεριφορές αποτελεσματική οδήγησης καυσίμου
- Η εκ των προτέρων μάθετε περισσότερα σχετικά με τους πελάτες και τους οδήγησης patters για διέπουν επιχειρηματικές αποφάσεις και παρέχει τις καλύτερες στο στοιχείο κατηγορίας προϊόντων και υπηρεσιών

Σε αυτήν τη λύση, θα σας στοχεύετε τα μετρικά παρακάτω:

1.  **Αυστηρές οδήγησης συμπεριφορά**: προσδιορίζει την τάση της μοντέλα, θέσεις, οδήγησης συνθήκες, και την ώρα του έτους για να αποκτήσετε ιδέες σε αυστηρές οδήγησης μοτίβα. Μηχανές Contoso να χρησιμοποιήσετε αυτές τις ιδέες για εκστρατείες μάρκετινγκ, οδήγησης νέες δυνατότητες εξατομικευμένης και χρήση βάσει ασφάλισης.
2.  **Αποτελεσματική συμπεριφορά οδήγησης καυσίμου**: προσδιορίζει την τάση της μοντέλα, θέσεις, οδήγησης συνθήκες, και την ώρα του έτους για να αποκτήσετε ιδέες σε πρότυπα οδήγησης αποτελεσματική καυσίμου. Μηχανές Contoso μπορεί να χρησιμοποιήσει αυτές τις ιδέες για μάρκετινγκ εκστρατείες, την νέες δυνατότητες και πριν από την ενεργοποίηση αναφοράς για τα προγράμματα οδήγησης για κόστους αποτελεσματική και περιβάλλον φιλικό οδήγησης οι συνήθειές όσον αφορά. 
3.  **Ανάκληση μοντέλα**: προσδιορίζει μοντέλα που απαιτεί υπενθυμίζει από operationalizing υπολογιστή εντοπισμού ανωμαλία εκμάθησης έρευνας

Ας ρίξουμε μια ματιά σε κάθε μία από αυτές τις μετρήσεις, τις λεπτομέρειες


**Αυστηρές οδήγησης μοτίβου**

Τα διαμερίσματα οχήματος σήματα και διαγνωστικών δεδομένων πραγματοποιείται στη διοχέτευση με το όνομα "AggresiveDrivingPatternPipeline" με χρήση της ομάδας για να προσδιορίσετε τα μοντέλα, θέση, οχήματος, συνθήκες οδήγησης και άλλες παραμέτρους που παρουσιάζει αυστηρές οδήγησης μοτίβο.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Σχήμα 20 – προοδευτική οδήγησης μοτίβο ροής εργασίας*

Η δέσμη ενεργειών ομάδα με το όνομα "aggresivedriving.hql" που χρησιμοποιείται για την ανάλυση αυστηρές οδήγησης μοτίβο συνθήκη βρίσκεται στο φάκελο "\demo\src\connectedcar\scripts" της λήψης zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Σχήμα 21 – προοδευτική οδήγησης μοτίβο Hive ερωτήματος*

Χρησιμοποιεί το συνδυασμό μετάδοση γραναζιού θέση του οχήματος, πέδης ποδόπληκτρο κατάσταση και την ταχύτητα για τον εντοπισμό reckless αυστηρές οδήγησης συμπεριφορά που βασίζονται σε πέδησης μοτίβο στο υψηλής ταχύτητας. 

Όταν η διαδικασία εκτελείται με επιτυχία, μπορείτε να δείτε τα ακόλουθα διαμερίσματα που δημιουργούνται στο λογαριασμό σας στο χώρο αποθήκευσης κάτω από το κοντέινερ "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Σχήμα 22 – AggressiveDrivingPatternPipeline εξόδου*


**Αποτελεσματική οδήγησης μοτίβο καυσίμου**

Τα διαμερίσματα οχήματος σήματα και διαγνωστικών δεδομένων πραγματοποιείται στη διοχέτευση με το όνομα "FuelEfficientDrivingPatternPipeline". Ομάδα χρησιμοποιείται για τον προσδιορισμό των μοντέλων, θέση, οχήματος, συνθήκες οδήγησης και άλλες ιδιότητες που παρουσιάζουν καυσίμου αποτελεσματική οδήγησης μοτίβο.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Σχήμα 23 – καυσίμου αποτελεσματική οδήγησης μοτίβο ροής εργασίας*

Η δέσμη ενεργειών ομάδα με το όνομα "fuelefficientdriving.hql" που χρησιμοποιείται για την ανάλυση αυστηρές οδήγησης μοτίβο συνθήκη βρίσκεται στο φάκελο "\demo\src\connectedcar\scripts" της λήψης zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Σχήμα 24 – καυσίμου αποτελεσματική οδήγησης μοτίβο Hive ερωτήματος*

Που χρησιμοποιεί ο συνδυασμός του οχήματος μετάδοση γραναζιού θέση, κατάσταση ποδόπληκτρο πέδησης, την ταχύτητα και επιτάχυνσης πεντάλ θέση για τον εντοπισμό καυσίμου αποτελεσματική συμπεριφορά οδήγησης επιτάχυνσης, πέδησης, με βάση και επιτάχυνση μοτίβα. 

Όταν η διαδικασία εκτελείται με επιτυχία, μπορείτε να δείτε τα ακόλουθα διαμερίσματα που δημιουργούνται στο λογαριασμό σας στο χώρο αποθήκευσης κάτω από το κοντέινερ "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Εικόνα 25-FuelEfficientDrivingPatternPipeline εξόδου*


**Ανάκληση προβλέψεων**

Το μηχανικής εκμάθησης έρευνας είναι παροχή της υπηρεσίας και να δημοσιευτεί ως υπηρεσία web ως μέρος του την ανάπτυξη της λύσης. Είναι δυνατή η δέσμη βαθμολογίας τελικού σημείου σε αυτήν τη ροή εργασίας, καταχωρημένος ως υπηρεσία δεδομένων εργοστασίου συνδεδεμένες και operationalized με χρήση δέσμης εργοστασίου δεδομένων βαθμολογίας δραστηριότητας.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Εικόνα 26 – μηχανικής εκμάθησης τελικού σημείου καταχωρηθεί ως συνδεδεμένο υπηρεσίας στην προέλευση δεδομένων*

Η υπηρεσία καταχωρημένες συνδεδεμένων χρησιμοποιείται σε το DetectAnomalyPipeline για τη βαθμολογία στα δεδομένα χρησιμοποιώντας το μοντέλο εντοπισμού ανωμαλία. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Εικόνα 27 – Azure μηχανικής εκμάθησης δέσμη βαθμολογίας δραστηριότητας στην προέλευση δεδομένων* 

Υπάρχουν μερικά βήματα που εκτελούνται σε αυτό διοχέτευσης για την προετοιμασία δεδομένων, έτσι ώστε το μπορεί να είναι operationalized με τη δέσμη βαθμολογίας υπηρεσία web. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Εικόνα 28 – DetectAnomalyPipeline για την πρόβλεψη οχήματα που απαιτεί υπενθυμίζει* 

Μόλις ολοκληρωθεί η βαθμολογίας, δραστηριότητα HDInsight χρησιμοποιείται για την επεξεργασία και να συγκεντρώσετε τα δεδομένα που έχουν κατηγοριοποιηθεί ως ανωμαλίες από το μοντέλο με βαθμολογία πιθανότητα 0.60 ή νεότερη έκδοση.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Όταν η διαδικασία εκτελείται με επιτυχία, μπορείτε να δείτε τα ακόλουθα διαμερίσματα που δημιουργούνται στο λογαριασμό σας στο χώρο αποθήκευσης κάτω από το κοντέινερ "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Εικόνα 30 – εικόνα 30 – DetectAnomalyPipeline εξόδου*


## <a name="publish"></a>Δημοσίευση

### <a name="real-time-analysis"></a>Ανάλυση σε πραγματικό χρόνο

Ένα από τα ερωτήματα της ροής εργασίας ανάλυση δημοσιεύει τα συμβάντα σε ένα αποτέλεσμα παρουσία διανομέα συμβάν. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Εικόνα 31-ροή δημοσιεύει ανάλυσης εργασίας για να το αποτέλεσμα παρουσία διανομέα συμβάντος*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Εικόνα 32-ροή ανάλυση ερωτήματος για να δημοσιεύσετε το αποτέλεσμα του συμβάντος διανομέα παρουσίας*

Αυτή η ροή συμβάντων καταναλώνεται από το RealTimeDashboardApp που περιλαμβάνονται στη λύση. Αυτή η εφαρμογή αξιοποιεί στην υπηρεσία web μηχανικής εκμάθησης αίτησης-απάντησης σε πραγματικό χρόνο βαθμολογίας και δημοσιεύει τα δεδομένα αποτελέσματος σε ένα σύνολο δεδομένων PowerBI για κατανάλωση. 

### <a name="batch-analysis"></a>Μαζική ανάλυση

Τα αποτελέσματα της δέσμης και σε πραγματικό χρόνο επεξεργασίας δημοσιεύονται σε πίνακες της βάσης δεδομένων SQL Azure για κατανάλωση. Το SQL Server Azure, βάση δεδομένων και τους πίνακες δημιουργούνται αυτόματα ως μέρος της ρύθμισης δέσμης ενεργειών. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Εικόνα 33 – μαζική επεξεργασία αντιγράφου αποτελεσμάτων με ροή εργασίας mart δεδομένων*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Εικόνα 34 – η ροή εργασίας ανάλυση δημοσιεύει σε mart δεδομένων*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Εικόνα 35-ρύθμιση σε ροή εργασίας ανάλυση mart δεδομένων*


## <a name="consume"></a>Εκμετάλλευση

Power BI παρέχουν αυτήν τη λύση εμπλουτισμένου πίνακα εργαλείων για δεδομένα σε πραγματικό χρόνο και απεικονίσεων πρόβλεψης ανάλυσης. 

Κάντε κλικ εδώ για λεπτομερείς οδηγίες σχετικά με τη ρύθμιση των τις αναφορές PowerBI και του πίνακα εργαλείων. Το τελικό πίνακα εργαλείων μοιάζει κάπως έτσι:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Εικόνα 36 - PowerBI πίνακα εργαλείων*

## <a name="summary"></a>Σύνοψη

Αυτό το έγγραφο περιέχει λεπτομερείς διερεύνηση της λύσης οχήματος Τηλεμετρίας ανάλυσης. Αυτό σάς δείχνουν ένα μοτίβο αρχιτεκτονική λάμδα για σε πραγματικό χρόνο και μαζικών ανάλυσης με προβλέψεων και ενέργειες. Αυτό το μοτίβο ισχύει για μια ευρεία ποικιλία περιπτώσεις χρήσης που απαιτούν συντόμευσης διαδρομή (σε πραγματικό χρόνο) και τις αναλύσεις ψυχρές διαδρομή (δέσμη). 
