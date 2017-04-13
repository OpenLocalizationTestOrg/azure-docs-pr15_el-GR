<properties
    pageTitle="Εφαρμογή Java στενής υπολογισμού σε μια Εικονική | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια Azure εικονικό μηχάνημα που εκτελεί μια εφαρμογή Java στενής υπολογισμού που μπορούν να παρακολουθούνται από άλλη εφαρμογή Java."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Πώς μπορείτε να εκτελέσετε μια εργασία υπολογισμού στενής σε Java σε μια εικονική μηχανή

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Με το Azure, μπορείτε να χρησιμοποιήσετε μια εικονική μηχανή χειρισμού υπολογισμού στενής εργασίες. Για παράδειγμα, μια εικονική μηχανή να χειριστείτε εργασίες και να κάνετε αποτελεσμάτων σε υπολογιστές-πελάτες ή εφαρμογές για κινητές συσκευές. Μετά την ανάγνωση αυτό το άρθρο, θα έχετε την κατανόηση του τρόπου για να δημιουργήσετε μια εικονική μηχανή που εκτελεί μια εφαρμογή Java στενής υπολογισμού που μπορούν να παρακολουθούνται από άλλη εφαρμογή Java.

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει που γνωρίζετε πώς μπορείτε να δημιουργήσετε εφαρμογές κονσόλας Java, να εισαγάγετε βιβλιοθήκες στην εφαρμογή σας Java και μπορεί να δημιουργήσει μια αρχειοθήκη Java (ΒΆΖΟ). Λαμβάνεται χωρίς γνώση του Windows Azure.

Θα μάθετε:

* Μάθετε πώς μπορείτε να δημιουργήσετε μια εικονική μηχανή με μια Java Development Kit (JDK) ήδη εγκατεστημένο.
* Μάθετε πώς μπορείτε να συνδεθείτε με την εικονική μηχανή από απόσταση.
* Μάθετε πώς μπορείτε να δημιουργήσετε ένα χώρο ονομάτων bus υπηρεσίας.
* Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή Java που εκτελεί μια εργασία στενής υπολογισμού.
* Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή Java που παρακολουθεί την πρόοδο της εργασίας στενής υπολογισμού.
* Μάθετε πώς μπορείτε να εκτελέσετε τις εφαρμογές Java.
* Μάθετε πώς μπορείτε να διακόψετε την εφαρμογές Java.

Αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσει το πρόβλημα Traveling πωλητή για την εργασία υπολογισμού στενής. Ακολουθεί ένα παράδειγμα της εφαρμογής Java που εκτελούνται στην εργασία στενής υπολογισμού.

![Επίλυση προβλήματος Traveling πωλητή][solver_output]

Ακολουθεί ένα παράδειγμα της εφαρμογής Java παρακολούθηση της εργασίας στενής υπολογισμού.

![Πρόβλημα πωλητή Traveling προγράμματος-πελάτη][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Για να δημιουργήσετε μια εικονική μηχανή

1. Συνδεθείτε στο [Azure κλασική πύλη](https://manage.windowsazure.com).
2. Κάντε κλικ στην επιλογή **Δημιουργία**, κάντε κλικ στην επιλογή **τον υπολογισμό**, κάντε κλικ στην επιλογή **εικονική μηχανή**και, στη συνέχεια, κάντε κλικ στην επιλογή **Από τη συλλογή**.
3. Στο παράθυρο διαλόγου **Επιλέξτε Εικόνα εικονική μηχανή** , επιλέξτε **JDK 7 Windows Server 2012**.
Σημειώστε ότι **JDK 6 Windows Server 2012** είναι διαθέσιμη σε περίπτωση που έχετε παλαιού τύπου εφαρμογές που δεν είναι ακόμη έτοιμη για να εκτελέσετε σε JDK 7.
4. Κάντε κλικ στο κουμπί **Επόμενο**.
4. Στο παράθυρο διαλόγου **Ρύθμιση παραμέτρων εικονική μηχανή** :
    1. Καθορίστε ένα όνομα για την εικονική μηχανή.
    2. Καθορίστε το μέγεθος για να χρησιμοποιήσετε για την εικονική μηχανή.
    3. Πληκτρολογήστε ένα όνομα για το διαχειριστή στο πεδίο **Όνομα χρήστη** . Να θυμάστε αυτό το όνομα και τον κωδικό πρόσβασης που θα εισαγάγετε στη συνέχεια, θα μπορείτε να τα χρησιμοποιήσετε όταν συνδέεστε απομακρυσμένα στο για να την εικονική μηχανή.
    4. Πληκτρολογήστε έναν κωδικό πρόσβασης στο πεδίο **νέο κωδικό πρόσβασης** και πληκτρολογήστε τον ξανά στο πεδίο **Επιβεβαίωση** . Πρόκειται για τον κωδικό πρόσβασης του λογαριασμού διαχειριστή.
    5. Κάντε κλικ στο κουμπί **Επόμενο**.
5. Στο επόμενο παράθυρο διαλόγου **Ρύθμιση παραμέτρων εικονική μηχανή** :
    1. Για την **υπηρεσία Cloud**, χρησιμοποιήστε την προεπιλεγμένη **Δημιουργία μια νέα υπηρεσία στο cloud**.
    2. Η τιμή για το **όνομα DNS υπηρεσία Cloud** πρέπει να είναι μοναδικό σε cloudapp.net. Εάν είναι απαραίτητο, τροποποιήστε αυτήν την τιμή, ώστε να Azure που δείχνει να είναι μοναδικό.
    2. Καθορίστε μια περιοχή, ομάδα συσχέτισης ή εικονικού δικτύου. Για σκοπούς αυτής της εκμάθησης, καθορίστε μια περιοχή όπως **Δυτική ΗΠΑ**.
    2. Για το **Λογαριασμό χώρου αποθήκευσης**, επιλέξτε **Χρήση λογαριασμού χώρου αποθήκευσης που δημιουργούνται αυτόματα**.
    3. **Ορισμός διαθεσιμότητας**, επιλέξτε **(καμία)**.
    4. Κάντε κλικ στο κουμπί **Επόμενο**.
5. Στο τελικό διαλόγου **Ρύθμιση παραμέτρων εικονική μηχανή** :
    1. Αποδεχτείτε τις προεπιλεγμένες καταχωρήσεις τελικού σημείου.
    2. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Για να συνδεθείτε από απόσταση στο την εικονική μηχανή

1. Συνδεθείτε στην [πύλη του Azure κλασική](https://manage.windowsazure.com).
2. Κάντε κλικ στην επιλογή **εικονικές μηχανές**.
3. Κάντε κλικ στο όνομα του υπολογιστή εικονικές που θέλετε να συνδεθείτε στο.
4. Κάντε κλικ στην επιλογή **σύνδεση**.
5. Απαντήστε στις ερωτήσεις όπως απαιτείται για να συνδεθείτε με την εικονική μηχανή. Όταν σας ζητηθεί το όνομα του διαχειριστή και τον κωδικό πρόσβασης, χρησιμοποιήστε τις τιμές που παρείχατε όταν δημιουργήσατε την εικονική μηχανή.

Σημειώστε ότι η λειτουργικότητα Azure Service Bus απαιτεί το πιστοποιητικό ρίζας CyberTrust Baltimore να έχει εγκατασταθεί ως μέρος του χώρου αποθήκευσης του JRE **cacerts** . Αυτό το πιστοποιητικό περιλαμβάνεται αυτόματα σε Java χρόνου εκτέλεσης περιβάλλον (JRE) χρησιμοποιείται από αυτό το πρόγραμμα εκμάθησης. Εάν δεν έχετε αυτό το πιστοποιητικό στο χώρο αποθήκευσης **cacerts** JRE, ανατρέξτε στο θέμα [Προσθήκη ένα πιστοποιητικό για την αποθήκευση του πιστοποιητικού Java CA] [ add_ca_cert] για πληροφορίες σχετικά με την προσθήκη του (καθώς και πληροφορίες σχετικά με την προβολή των πιστοποιητικών στο χώρο αποθήκευσης cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Πώς μπορείτε να δημιουργήσετε ένα χώρο ονομάτων bus υπηρεσίας

Για να ξεκινήσετε να χρησιμοποιείτε ουρές Bus υπηρεσίας στο Azure, πρέπει πρώτα να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας. Ένα χώρο ονομάτων υπηρεσίας παρέχει ενός πεδίου κοντέινερ για προσθήκη διεύθυνσης σε πόρους Bus υπηρεσίας στην εφαρμογή σας.

Για να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας:

1.  Συνδεθείτε στην [πύλη του Azure κλασική](https://manage.windowsazure.com).
2.  Στο παράθυρο περιήγησης κάτω αριστερά από το Azure κλασική πύλη, κάντε κλικ στην επιλογή **υπηρεσία Bus, έλεγχος πρόσβασης & προσωρινή αποθήκευση**.
3.  Στο παράθυρο της πύλης Azure κλασική επάνω αριστερά, κάντε κλικ στον κόμβο **Bus υπηρεσίας** και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημιουργία** .  
    ![Στιγμιότυπο οθόνης της υπηρεσίας Bus κόμβου][svc_bus_node]
4.  Στο παράθυρο διαλόγου **Δημιουργία ενός νέου Namespace υπηρεσίας** , πληκτρολογήστε μια **Namespace**και, στη συνέχεια, για να βεβαιωθείτε ότι είναι μοναδικά, κάντε κλικ στο κουμπί **Έλεγχος διαθεσιμότητας** .  
    ![Δημιουργήστε ένα στιγμιότυπο οθόνης νέου Namespace][create_namespace]
5.  Αφού και βεβαιωθείτε ότι το όνομα χώρου ονομάτων είναι διαθέσιμη, επιλέξτε τη χώρα ή την περιοχή στην οποία πρέπει να φιλοξενούνται σας χώρο ονομάτων και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημιουργία Namespace** .  

    Το πεδίο ονόματος που δημιουργήσατε, στη συνέχεια, θα εμφανιστεί στην πύλη του Azure κλασική και χρειάζεται λίγο χρόνο για να ενεργοποιήσετε. Περιμένετε έως ότου η κατάσταση είναι **ενεργό** πριν να συνεχίσετε με το επόμενο βήμα.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Αποκτήστε τα διαπιστευτήρια διαχείρισης προεπιλεγμένη για το χώρο ονομάτων

Για να εκτελέσετε εργασίες διαχείρισης, όπως τη δημιουργία μιας ουράς, σε νέο χώρο ονομάτων, πρέπει να λάβετε τα διαπιστευτήρια διαχείρισης για το χώρο ονομάτων.

1.  Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στον κόμβο **Bus υπηρεσίας** για να εμφανιστεί η λίστα διαθέσιμα πεδία ονομάτων.
    ![Στιγμιότυπο οθόνης διαθέσιμα πεδία ονομάτων][avail_namespaces]
2.  Επιλέξτε το πεδίο ονόματος που μόλις δημιουργήσατε από τη λίστα που εμφανίζεται.
    ![Στιγμιότυπο οθόνης Namespace λίστας][namespace_list]
3.  Δεξιό παράθυρο **Ιδιότητες** παραθέτει τις ιδιότητες για το νέο χώρο ονομάτων.
    ![Στιγμιότυπο οθόνης του παραθύρου ιδιοτήτων][properties_pane]
4.  Το **Προεπιλεγμένο πλήκτρο** είναι ορατή. Κάντε κλικ στο κουμπί **Προβολή** για να εμφανίσετε τα διαπιστευτήρια ασφαλείας.
    ![Στιγμιότυπο οθόνης του προεπιλεγμένου αριθμού-κλειδιού][default_key]
5.  Σημειώστε τον **Προεπιλεγμένο εκδότη** και το **Πλήκτρο προεπιλεγμένη** όπως θα μπορείτε να χρησιμοποιήσετε τις παρακάτω πληροφορίες για την εκτέλεση λειτουργιών με το χώρο ονομάτων.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Java που εκτελεί μια εργασία στενής υπολογισμού

1. Στον υπολογιστή σας ανάπτυξης (το οποίο δεν χρειάζεται να είναι η εικονική μηχανή που έχετε δημιουργήσει), κάντε λήψη του [Azure SDK για Java](https://azure.microsoft.com/develop/java/).
2. Δημιουργία μιας εφαρμογής κονσόλας Java χρησιμοποιώντας τον κωδικό παράδειγμα στο τέλος αυτής της ενότητας. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε **TSPSolver.java** ως το όνομα του αρχείου Java της. Τροποποίηση του **σας\_υπηρεσία\_bus\_χώρο ονομάτων**, **σας\_υπηρεσία\_bus\_κάτοχο**, και **σας\_υπηρεσίας\_bus\_αριθμού-κλειδιού** σύμβολα κράτησης θέσης για να χρησιμοποιήσετε την υπηρεσία δίαυλο **χώρο ονομάτων**, **Προεπιλεγμένες εκδότη** και **Κλειδί προεπιλεγμένες** τιμές, αντίστοιχα.
3. Μετά την κωδικοποίηση, εξαγωγή της εφαρμογής σε μια αρχειοθήκη εκτελέσιμες Java (ΒΆΖΟ) και πακέτο τις απαιτούμενες βιβλιοθήκες σε το ΒΆΖΟ που δημιουργήθηκε. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε **TSPSolver.jar** ως το όνομα που έχει δημιουργηθεί ΒΆΖΟ της.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Java που παρακολουθεί την πρόοδο της εργασίας στενής υπολογισμού

1. Στον υπολογιστή σας ανάπτυξης, δημιουργία εφαρμογής κονσόλας Java χρησιμοποιώντας το παράδειγμα κώδικα στο τέλος αυτής της ενότητας. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε **TSPClient.java** ως το όνομα του αρχείου Java της. Όπως φαίνεται παραπάνω, τροποποιήστε το **σας\_υπηρεσία\_bus\_χώρο ονομάτων**, **σας\_υπηρεσίας\_bus\_κάτοχο**, και **σας\_υπηρεσίας\_bus\_αριθμού-κλειδιού** σύμβολα κράτησης θέσης για να χρησιμοποιήσετε την υπηρεσία δίαυλο **χώρο ονομάτων**, **Προεπιλεγμένες εκδότη** και **Κλειδί προεπιλεγμένες** τιμές, αντίστοιχα.
2. Εξαγωγή της εφαρμογής σε μια εκτελέσιμες ΒΆΖΟ και πακέτο τις απαιτούμενες βιβλιοθήκες σε το ΒΆΖΟ που δημιουργήθηκε. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε **TSPClient.jar** ως το όνομα που έχει δημιουργηθεί ΒΆΖΟ της.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Πώς μπορείτε να εκτελέσετε τις εφαρμογές Java
Εκτελέστε την εφαρμογή υπολογισμού στενής, πρώτα, για να δημιουργήσετε την ουρά, στη συνέχεια, να επιλύσετε το πρόβλημα Saleseman ταξιδεύετε, θα προστεθεί η τρέχουσα καλύτερη δρομολόγηση στην ουρά bus υπηρεσίας. Ενώ η εφαρμογή στενής υπολογισμού είναι τρέχον (ή αργότερα), εκτελέστε το πρόγραμμα-πελάτη για να εμφανίσετε τα αποτελέσματα από την υπηρεσία ουράς bus.

### <a name="to-run-the-compute-intensive-application"></a>Για να εκτελέσετε την εφαρμογή στενής υπολογισμού

1. Συνδεθείτε στον υπολογιστή σας εικονική.
2. Δημιουργήστε ένα φάκελο στον οποίο θα εκτελείτε την εφαρμογή σας. Για παράδειγμα, **c:\TSP**.
3. Αντιγραφή **TSPSolver.jar** **c:\TSP**,
4. Δημιουργήστε ένα αρχείο με το όνομα **c:\TSP\cities.txt** με το παρακάτω περιεχόμενο.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Σε μια γραμμή εντολών, αλλάξτε σε καταλόγους c:\TSP.
6. Βεβαιωθείτε ότι το JRE Ανακύκλωσης φάκελος είναι σε μεταβλητή περιβάλλοντος PATH.
7. Θα χρειαστεί να δημιουργήσετε την υπηρεσία ουρά bus πριν να εκτελέσετε το διατάξεων επίλυσης TSP. Εκτελέστε την παρακάτω εντολή για να δημιουργήσετε την υπηρεσία ουράς bus.

        java -jar TSPSolver.jar createqueue

8. Τώρα που δημιουργείται στην ουρά, μπορείτε να εκτελέσετε το TSP διατάξεων επίλυσης. Για παράδειγμα, εκτελέστε την ακόλουθη εντολή για να εκτελέσετε το επίλυσης για 8 πόλεις.

        java -jar TSPSolver.jar 8

 Εάν δεν καθορίσετε έναν αριθμό, θα εκτελείται για 10 πόλεις. Κατά την τρέχουσα συντομότερη δρομολογεί εύρεσης η επίλυση, θα προσθέσει τους στην ουρά.

> [AZURE.NOTE]
> Τόσο μεγαλύτερη τον αριθμό που καθορίζετε, περισσότερο θα εκτελεστεί η επίλυση. Για παράδειγμα, εκτελεί για 14 πόλεις μπορεί να χρειαστούν αρκετά λεπτά και να εκτελείται για 15 πόλεις μπορεί να διαρκέσει μερικές ώρες. Αύξηση σε 16 ή περισσότερες πόλεις μπορεί να οδηγήσει σε ημέρες του χρόνου εκτέλεσης (τελικά εβδομάδες, μήνες και έτη). Αυτό οφείλεται η ταχεία αύξηση στον αριθμό των διατάξεων που υπολογίζεται από την επίλυση ως ο αριθμός των πόλεων αυξάνεται.

### <a name="how-to-run-the-monitoring-client-application"></a>Πώς μπορείτε να εκτελέσετε την εφαρμογή-πελάτη παρακολούθησης
1. Συνδεθείτε στον υπολογιστή σας, όπου θα εκτελείτε την εφαρμογή-πελάτη. Δεν χρειάζεται να είναι το ίδιο υπολογιστή που εκτελεί την εφαρμογή **TSPSolver** , παρόλο που μπορεί να είναι.
2. Δημιουργήστε ένα φάκελο στον οποίο θα εκτελείτε την εφαρμογή σας. Για παράδειγμα, **c:\TSP**.
3. Αντιγραφή **TSPClient.jar** **c:\TSP**,
4. Βεβαιωθείτε ότι το JRE Κάδο φάκελος είναι σε μεταβλητή περιβάλλοντος PATH.
5. Σε μια γραμμή εντολών, αλλάξτε σε καταλόγους c:\TSP.
6. Εκτελέστε την ακόλουθη εντολή.

        java -jar TSPClient.jar

    Προαιρετικά, καθορίστε τον αριθμό των λεπτών σε αναστολή λειτουργίας ανάμεσα στα έλεγχος ουρά, περνώντας σε ένα όρισμα γραμμής εντολών. Η προεπιλεγμένη περίοδος αναμονής για τον έλεγχο της ουράς είναι 3 λεπτά, που χρησιμοποιείται εάν δεν έχει δοθεί όρισμα γραμμής εντολών που του μεταβιβάστηκε **TSPClient**. Εάν θέλετε να χρησιμοποιήσετε μια διαφορετική τιμή για το χρονικό διάστημα αναμονής, για παράδειγμα, ένα λεπτό, εκτελέστε την ακόλουθη εντολή.

        java -jar TSPClient.jar 1

    Ο υπολογιστής-πελάτης θα εκτελεστεί έως ότου το βλέπει ένα μήνυμα ουρά "Ολοκληρώθηκε". Λάβετε υπόψη ότι εάν εκτελείτε πολλές εμφανίσεις της επίλυσης το χωρίς την εκτέλεση του προγράμματος-πελάτη, ίσως χρειαστεί να εκτελέσετε το πρόγραμμα-πελάτη πολλές φορές για να αδειάσετε εντελώς ουρά. Εναλλακτικά, μπορείτε να διαγράψετε την ουρά και να δημιουργήσετε ξανά. Για να διαγράψετε την ουρά, εκτελέστε την ακόλουθη εντολή **TSPSolver** (δεν **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Η "Επίλυση" θα εκτελεστεί μέχρι να ολοκληρωθεί η εξέταση όλες διαδρομές.

## <a name="how-to-stop-the-java-applications"></a>Πώς μπορείτε να σταματήσετε τις εφαρμογές Java
Τόσο για την επίλυση και εφαρμογές προγράμματος-πελάτη, μπορείτε να πατήσετε το **Συνδυασμό πλήκτρων Ctrl + C** για να εξέλθετε από το εάν θέλετε να τερματίσετε πριν από την κανονική ολοκλήρωσης.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
